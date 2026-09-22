# Tests for a complete typed integration

This reference is standalone: it states the production contracts that the examples exercise. Imports below refer to the application being tested, not files in another skill. Keep equivalent tests next to their feature in your project; do not create a shared testing framework merely to copy the layout.

## Contract and collaborators

`stock_sync.application.Observation(sku: str, quantity: int)` and `SyncResult(snapshot_id: str, accepted_count: int)` are frozen dataclasses. `SyncCommand(skus: tuple[str, ...])` accepts 1–100 nonblank distinct identifiers. `InventorySync(reader, writer, concurrency)` consumes `reader.read(sku) -> Observation` and `writer.record(tuple[Observation, ...]) -> SyncResult`. Its `run(command)` bounds simultaneous reads per run with a semaphore inside a TaskGroup, preserves command order, and calls the writer once only after every read succeeds. Failed reads cancel and await siblings. No deletion or retry exists; a failed write may already have committed.

`ProviderProduct` is a Pydantic external model with forbidden extras, strict nonempty string `sku`, strict nonnegative integer `available`. `to_observation` explicitly maps available units to `quantity`. Zero is legitimate; absent, null, numeric string, boolean and negative values are invalid. `write_csv(items, stream)` uses csv.writer and leaves the caller-owned stream open; it targets machine CSV, not spreadsheet formula safety.

`load_settings(explicit_env_file)` uses Pydantic Settings. `SYNC_` environment values override the selected local file. Timeout is finite and positive, concurrency is 1–20, token is nonempty and secret-redacted. `execute(command, env_file, provider_transport=..., inventory_transport=...)` validates settings **before constructing HTTP clients**, composes the real adapters, and owns both client closures. `main()` returns 1 with the safe ConfigurationError diagnostic or a fixed operational message. ConfigurationError preserves its private cause. SourceReadError and SnapshotWriteError translate provider and generated-client failures with `raise ... from error`; read failures appear in the TaskGroup ExceptionGroup. Neither adapter catches cancellation, and the CLI never prints payload-bearing causes or tracebacks.

Provider HTTP is `GET /products?sku=A` with JSON `{ "sku": "A", "available": 4 }`. The generated owned API is OpenAPI Generator **7.22.0**, `python` library `httpx`, package `inventory_client`: `InventoryApi.create_inventory_snapshot(CreateSnapshotRequest(...))` posts to `/inventory-snapshots`, authenticates with bearer auth and returns `SnapshotReceiptResponse`. Request JSON is `{ "items": [{ "sku": "A", "quantity": 4 }] }`; all fields required. The flat 201 receipt requires nonempty string `snapshotId`, strict integer `acceptedCount >= 1`, and enum `status: "recorded"`; none is nullable and extras are forbidden. A 503 is an error, never a receipt. The supplied OpenAPI operation explicitly requires an atomic write; that server guarantee is assumed by this client and is not proven by MockTransport. The adapter also rejects a count inconsistent with the submitted observation count.

The actual generated 7.22.0 deserializer drops unknown properties despite the contract. The raw-path characterization below deliberately proves that limitation; it is **not** a conformance test. Production composition installs a narrow HTTPX hook that rejects extra receipt fields using allowed aliases derived from generated model metadata; other validation remains on the real generated decode path. No DTO mirror, handwritten client stub or patched generated source is involved. `GeneratedSnapshotWriter` by itself borrows a configured API; the guarded production boundary is `execute`. After an unusable receipt, do not claim rollback or retry automatically.

## Running and interpreting the tests

The project needs Python 3.12, httpx 0.28.1, Pydantic 2.13.5, pydantic-settings 2.15.0 and python-dateutil 2.9.0.post0; tests use pytest 9.1.1 and pytest-asyncio 1.4.0. Generate the contract client into `.generated` with OpenAPI Generator 7.22.0 before collection. Configure pytest `pythonpath = ["src", ".generated"]`, `asyncio_mode = "auto"`, `testpaths = ["tests"]`. Configure mypy `strict = true`, `files = ["src", "tests"]`, `mypy_path = "src:.generated"`, `plugins = ["pydantic.mypy"]`, with `init_typed = true` for the plugin. A **generated-module-only** override `module = ["inventory_client.*"]`, `follow_imports = "silent"` retains imported annotations while excluding generator internals from handwritten checks. No global missing-import suppression or replacement stubs are used.

Run `uv run pytest -q`, `uv run mypy`, `uv run ruff check src tests` and `uv run ruff format --check src tests`. The checked example produced **34 passing cases**, strict typing passed for all 7 handwritten source/test files, and Ruff 0.16.8 and mypy 2.3.1 passed. This checks the actual generated operation through HTTPX MockTransport: request serialization, header/path/timeout behavior and response deserialization execute unchanged. Transport control is not evidence of live server persistence or atomicity.

Small inputs remain visible at each scenario. RecordingWriter and FixedReader are narrow use-case fakes; the integration tests instantiate real generated objects. Events establish ordering for concurrency and cancellation; an outer timeout merely prevents a broken test from hanging. Each created task is awaited or cancelled and awaited. No sleeps, production traffic, random state, nested event loops in async tests or hidden factory writes occur. Faker is omitted because secondary randomized fields add no value to these records. For richer reused test data, use a typed factory with a local fixed Faker seed and explicit decisive overrides; do not turn these tiny records into a generic object factory.

## `tests/test_sync.py`

```python
"""Behavior tests: real values, narrow fakes, and real generated HTTPX operations."""

import asyncio
import csv
import io
import json
from dataclasses import dataclass, field
from pathlib import Path

import httpx
import pytest
from inventory_client.api.inventory_api import InventoryApi
from inventory_client.api_client import ApiClient
from inventory_client.configuration import Configuration
from pydantic import ValidationError

from stock_sync.adapters import (
    GeneratedSnapshotWriter,
    ProviderProduct,
    to_observation,
)
from stock_sync.application import (
    InventorySync,
    Observation,
    SnapshotWriteError,
    SourceReadError,
    SyncCommand,
    SyncResult,
)
from stock_sync.config import ConfigurationError, load_settings
from stock_sync.csv_report import write_csv
from stock_sync.main import execute, main


@pytest.fixture
def env_file(tmp_path: Path, monkeypatch: pytest.MonkeyPatch) -> Path:
    # Each test owns an explicit local config; no network or personal credentials.
    import os

    for key in os.environ:
        if key.startswith("SYNC_"):
            monkeypatch.delenv(key)
    path = tmp_path / ".env.local"
    path.write_text(
        "SYNC_PROVIDER_URL=https://provider.example.test/\n"
        "SYNC_INVENTORY_URL=https://inventory.example.test\n"
        "SYNC_TOKEN=fixture-secret\n"
        "SYNC_TIMEOUT_SECONDS=2\n"
        "SYNC_CONCURRENCY=2\n"
    )
    return path


class ClosingTransport(httpx.MockTransport):
    closed = False

    async def aclose(self) -> None:
        self.closed = True
        await super().aclose()


def provider_response(request: httpx.Request) -> httpx.Response:
    return httpx.Response(200, json={"sku": request.url.params["sku"], "available": 4})


def receipt_response(request: httpx.Request) -> httpx.Response:
    return httpx.Response(
        201, json={"snapshotId": "s-1", "acceptedCount": 1, "status": "recorded"}
    )


@dataclass
class RecordingWriter:
    calls: list[tuple[Observation, ...]] = field(default_factory=list)

    async def record(self, items: tuple[Observation, ...]) -> SyncResult:
        self.calls.append(items)
        return SyncResult(f"s-{len(self.calls)}", len(items))


class FixedReader:
    async def read(self, sku: str) -> Observation:
        return Observation(sku, 4)


def test_explicit_mapping_preserves_zero_and_case() -> None:
    # Given
    product = ProviderProduct(sku="Ab-1", available=0)

    # When
    observation = to_observation(product)

    # Then
    assert observation == Observation("Ab-1", 0)


@pytest.mark.parametrize("available", ["4", True, -1, None])
def test_provider_does_not_coerce_invalid_json_integer(available: object) -> None:
    # Given
    payload = json.dumps({"sku": "A", "available": available})

    # When / Then
    with pytest.raises(ValidationError):
        ProviderProduct.model_validate_json(payload)


def test_csv_quotes_delimiters_without_owning_stream() -> None:
    # Given
    stream = io.StringIO(newline="")

    # When
    write_csv((Observation('A,"B', 0),), stream)

    # Then
    stream.seek(0)
    assert list(csv.reader(stream)) == [["sku", "quantity"], ['A,"B', "0"]]
    assert not stream.closed


async def test_successive_runs_do_not_reuse_observations() -> None:
    # Given: same application instance and collaborators, two commands.
    writer = RecordingWriter()
    use_case = InventorySync(FixedReader(), writer, concurrency=2)

    # When
    first = await use_case.run(SyncCommand(("A",)))
    second = await use_case.run(SyncCommand(("B",)))

    # Then
    assert first == SyncResult("s-1", 1)
    assert second == SyncResult("s-2", 1)
    assert writer.calls == [(Observation("A", 4),), (Observation("B", 4),)]


async def test_bounded_reads_keep_command_order() -> None:
    # Given: an event barrier holds the first two reads; no timing sleeps.
    release = asyncio.Event()
    full = asyncio.Event()
    active = 0
    peak = 0

    class BlockingReader:
        async def read(self, sku: str) -> Observation:
            nonlocal active, peak
            active += 1
            peak = max(peak, active)
            if active == 2:
                full.set()
            try:
                await release.wait()
                return Observation(sku, 1)
            finally:
                active -= 1

    writer = RecordingWriter()
    use_case = InventorySync(BlockingReader(), writer, concurrency=2)

    # When
    async with asyncio.timeout(2):
        task = asyncio.create_task(use_case.run(SyncCommand(("A", "B", "C"))))
        try:
            await full.wait()
            assert active == 2
            release.set()
            result = await task
        finally:
            task.cancel()
            await asyncio.gather(task, return_exceptions=True)

    # Then
    assert peak == 2
    assert active == 0
    assert result.accepted_count == 3
    assert writer.calls == [
        (Observation("A", 1), Observation("B", 1), Observation("C", 1))
    ]


async def test_failed_read_cancels_sibling_and_never_writes() -> None:
    # Given: the failing read waits until its sibling has acquired resources.
    started = asyncio.Event()
    closed = asyncio.Event()

    class FailingReader:
        async def read(self, sku: str) -> Observation:
            if sku == "bad":
                await started.wait()
                raise ValueError("incomplete external data")
            started.set()
            try:
                await asyncio.Event().wait()
            finally:
                closed.set()
            raise AssertionError("unreachable")

    writer = RecordingWriter()
    use_case = InventorySync(FailingReader(), writer, concurrency=2)

    # When
    with pytest.raises(ExceptionGroup) as failure:
        async with asyncio.timeout(2):
            await use_case.run(SyncCommand(("bad", "waiting")))

    # Then
    assert any(isinstance(error, ValueError) for error in failure.value.exceptions)
    assert closed.is_set()
    assert writer.calls == []


async def test_real_generated_request_and_cleanup(env_file: Path) -> None:
    # Given: real generated operation, DTOs, serializer and response decoder;
    # only HTTP transport is controlled.
    requests: list[httpx.Request] = []

    def inventory_response(request: httpx.Request) -> httpx.Response:
        requests.append(request)
        return receipt_response(request)

    provider = ClosingTransport(provider_response)
    inventory = ClosingTransport(inventory_response)

    # When
    result = await execute(
        SyncCommand(("A",)),
        env_file,
        provider_transport=provider,
        inventory_transport=inventory,
    )

    # Then
    assert result == SyncResult("s-1", 1)
    (request,) = requests
    assert request.method == "POST"
    assert str(request.url) == "https://inventory.example.test/inventory-snapshots"
    assert request.headers["authorization"] == "Bearer fixture-secret"
    assert json.loads(request.content) == {"items": [{"sku": "A", "quantity": 4}]}
    assert request.extensions["timeout"]["read"] == 2.0
    assert provider.closed and inventory.closed


@pytest.mark.parametrize(
    "payload",
    [
        {},
        {"snapshotId": "s", "acceptedCount": "1", "status": "recorded"},
        {"snapshotId": "s", "acceptedCount": 1, "status": "unknown"},
        {"snapshotId": None, "acceptedCount": 1, "status": "recorded"},
        {"snapshotId": "s", "acceptedCount": 1, "status": "recorded", "extra": True},
        {"snapshotId": "s", "acceptedCount": 2, "status": "recorded"},
        None,
        [],
    ],
)
async def test_actual_generated_boundary_rejects_bad_receipts(
    env_file: Path, payload: object
) -> None:
    # Given
    provider = ClosingTransport(provider_response)
    inventory = ClosingTransport(lambda request: httpx.Response(201, json=payload))

    # When / Then: validation fails after write; no rollback/retry is claimed.
    with pytest.raises(SnapshotWriteError) as failure:
        await execute(
            SyncCommand(("A",)),
            env_file,
            provider_transport=provider,
            inventory_transport=inventory,
        )
    assert isinstance(failure.value.__cause__, ValueError)
    assert provider.closed and inventory.closed


async def test_unpatched_generated_decoder_drops_extra_properties() -> None:
    # Given: deliberately omit the narrow guard to characterize generator 7.22.0.
    def response(request: httpx.Request) -> httpx.Response:
        return httpx.Response(
            201,
            json={
                "snapshotId": "s",
                "acceptedCount": 1,
                "status": "recorded",
                "forbidden": "silently dropped",
            },
        )

    async with httpx.AsyncClient(transport=httpx.MockTransport(response)) as http:
        api_client = ApiClient(Configuration(host="https://inventory.example.test"))
        api_client.rest_client.pool_manager = http
        writer = GeneratedSnapshotWriter(InventoryApi(api_client), 2.0)

        # When
        result = await writer.record((Observation("A", 1),))

    # Then: this proves a gap, not contract conformance.
    assert result == SyncResult("s", 1)


@pytest.mark.parametrize(
    "payload",
    [
        {},
        {"sku": "A"},
        {"sku": "A", "available": None},
        {"sku": "A", "available": "4"},
        {"sku": "wrong", "available": 4},
    ],
)
async def test_partial_provider_data_never_reaches_write(
    env_file: Path, payload: object
) -> None:
    # Given
    writes: list[httpx.Request] = []

    def write(request: httpx.Request) -> httpx.Response:
        writes.append(request)
        return receipt_response(request)

    provider = ClosingTransport(lambda request: httpx.Response(200, json=payload))
    inventory = ClosingTransport(write)

    # When / Then
    with pytest.raises(ExceptionGroup) as failure:
        await execute(
            SyncCommand(("A",)),
            env_file,
            provider_transport=provider,
            inventory_transport=inventory,
        )
    assert all(isinstance(error, SourceReadError) for error in failure.value.exceptions)
    assert all(error.__cause__ is not None for error in failure.value.exceptions)
    assert writes == []
    assert provider.closed and inventory.closed


async def test_cancellation_closes_clients_without_write(env_file: Path) -> None:
    # Given
    started = asyncio.Event()
    writes: list[httpx.Request] = []

    async def wait(request: httpx.Request) -> httpx.Response:
        started.set()
        await asyncio.Event().wait()
        raise AssertionError("unreachable")

    def write(request: httpx.Request) -> httpx.Response:
        writes.append(request)
        return receipt_response(request)

    provider = ClosingTransport(wait)
    inventory = ClosingTransport(write)

    # When
    async with asyncio.timeout(2):
        task = asyncio.create_task(
            execute(
                SyncCommand(("A",)),
                env_file,
                provider_transport=provider,
                inventory_transport=inventory,
            )
        )
        try:
            await started.wait()
            task.cancel()
            with pytest.raises(asyncio.CancelledError):
                await task
        finally:
            task.cancel()
            await asyncio.gather(task, return_exceptions=True)

    # Then
    assert writes == []
    assert provider.closed and inventory.closed


def test_environment_overrides_explicit_local_file(
    env_file: Path, monkeypatch: pytest.MonkeyPatch
) -> None:
    # Given
    monkeypatch.setenv("SYNC_CONCURRENCY", "3")

    # When
    settings = load_settings(env_file)

    # Then
    assert settings.concurrency == 3
    assert settings.timeout_seconds == 2.0
    assert "fixture-secret" not in repr(settings)


@pytest.mark.parametrize(
    "name,value",
    [
        ("SYNC_TIMEOUT_SECONDS", "0"),
        ("SYNC_CONCURRENCY", "-1"),
        ("SYNC_TIMEOUT_SECONDS", "nan"),
        ("SYNC_TOKEN", ""),
    ],
)
async def test_invalid_configuration_precedes_client_construction(
    env_file: Path, monkeypatch: pytest.MonkeyPatch, name: str, value: str
) -> None:
    # Given
    monkeypatch.setenv(name, value)
    constructed = False

    def unexpected_client(*args: object, **kwargs: object) -> None:
        nonlocal constructed
        constructed = True
        raise AssertionError("Client constructed before configuration validation")

    monkeypatch.setattr(httpx, "AsyncClient", unexpected_client)

    # When / Then
    with pytest.raises(
        ConfigurationError, match="Invalid synchronization configuration"
    ) as failure:
        await execute(SyncCommand(("A",)), env_file)
    assert isinstance(failure.value.__cause__, ValidationError)
    assert not constructed


def test_cli_error_does_not_expose_configuration_secret(
    env_file: Path, monkeypatch: pytest.MonkeyPatch, capsys: pytest.CaptureFixture[str]
) -> None:
    # Given
    monkeypatch.chdir(env_file.parent)
    monkeypatch.setattr("sys.argv", ["stock-sync", "A"])
    monkeypatch.setenv("SYNC_TIMEOUT_SECONDS", "secret-invalid-value")

    # When
    exit_code = main()

    # Then
    error = capsys.readouterr().err
    assert exit_code == 1
    assert "secret-invalid-value" not in error
    assert "fixture-secret" not in error
    assert "Invalid synchronization configuration" in error


@pytest.mark.parametrize("mode", ["timeout", "unavailable", "malformed"])
async def test_write_failure_propagates_once_and_closes_clients(
    env_file: Path, mode: str
) -> None:
    # Given: a POST may have committed even when its response is unusable.
    calls = 0

    def failure(request: httpx.Request) -> httpx.Response:
        nonlocal calls
        calls += 1
        if mode == "timeout":
            raise httpx.ReadTimeout("private diagnostic", request=request)
        if mode == "unavailable":
            return httpx.Response(
                503,
                json={
                    "type": "https://example.test/problems/unavailable",
                    "title": "Unavailable",
                    "status": 503,
                    "code": "unavailable",
                },
            )
        return httpx.Response(201, content=b'{"snapshotId":')

    provider = ClosingTransport(provider_response)
    inventory = ClosingTransport(failure)

    # When / Then
    from inventory_client.exceptions import ApiException

    with pytest.raises(SnapshotWriteError) as error:
        await execute(
            SyncCommand(("A",)),
            env_file,
            provider_transport=provider,
            inventory_transport=inventory,
        )
    assert isinstance(
        error.value.__cause__, (httpx.ReadTimeout, ApiException, ValueError)
    )
    assert "private diagnostic" not in str(error.value)
    assert calls == 1
    assert provider.closed and inventory.closed
```
