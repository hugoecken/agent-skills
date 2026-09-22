# Small complete Python integration example

Use this as an architecture example when adding a real integration, not as a mandatory starter tree. An inventory observation is deliberately a general backend workflow: read selected products from an external JSON API, validate, map, record a snapshot through an owned generated OpenAPI client, return a typed receipt, and close resources. The CSV export is a small pure formatting operation; the [standalone script](simple-script.md) separately shows a complete CLI without application layers.

## Behavior and ownership

The immutable `SyncCommand` selects 1–100 unique product identifiers. `InventorySync` has constructor-injected consumer protocols and no mutable per-run state. Each run limits active reads, waits for all of them, then performs one non-destructive POST whose atomicity is explicitly required by the supplied OpenAPI contract. Atomicity is an expected server guarantee, not something MockTransport proves. It never deletes missing products. Provider absence, partial records, wrong identifiers, read failure or cancellation prevent any write. Failed reads cancel and await sibling tasks. The command bounds task count as well as the semaphore bounding active I/O.

This is a limited ingestion safety variant, not a scheduler or reconciliation framework. It has no framework, ORM, container, retry library, automatic retry or deletion protocol. A failed/cancelled POST or invalid receipt may follow a committed write: caller reconciliation is required before another attempt; client code cannot promise distributed rollback. Concurrency is bounded **per run**, not globally across concurrent callers. Add an application-wide owner only if the service requires a global bound.

Provider JSON has no owned OpenAPI contract, so a small Pydantic model validates it. Integer `available` rejects strings, booleans, null and negatives; unknown keys fail. Internal commands, observations and results are frozen dataclasses. Frozen is shallow, so the command uses a tuple. The owned write API uses real OpenAPI Generator 7.22.0 `python` / `httpx` output. Pure mapping functions convert provider values and generated request/response objects at the adapter boundary. There is no handwritten API DTO family.

Settings intentionally coerce environment strings to numbers; this does **not** permit coercion of JSON integers. `load_settings` runs before either HTTP client is constructed. Environment variables override the explicitly supplied `.env.local`, then defaults. No env file is discovered implicitly. Timeout must be finite and positive; concurrency is 1–20; the secret must be nonempty. Environment configuration uses normal Pydantic Settings sources; constructor values are not exposed by `load_settings`. Do not print raw validation errors, generated API exceptions, response payloads, settings values or chained tracebacks at the terminal boundary. The CLI prints the safe ConfigurationError diagnostic or a fixed operational failure message without traceback. ConfigurationError preserves its Pydantic/OSError cause. The provider adapter translates HTTPX/validation errors into SourceReadError; the write adapter translates HTTPX/generated-client/validation errors into SnapshotWriteError. Both use `raise ... from error`, preserve private diagnostic causes and never catch cancellation. Application callers receive these targeted exceptions (read failures are grouped by TaskGroup), not raw transport failures.

## Proven generator limitation

In 7.22.0 the generated `SnapshotReceiptResponse.from_dict` selects known fields and silently drops forbidden extra properties despite `additionalProperties: false`. Its Pydantic configuration also lacks `extra="forbid"`. A passing generated DTO import is not proof of wire validation.

The small HTTPX response hook rejects unknown keys for this **flat 201 receipt**, deriving allowed wire names from generated model metadata. It does not copy the schema, modify generated files or implement a second DTO. The actual generated deserializer still validates required fields, strict types, enum values and non-null fields. The hook also rejects non-object success bodies. This is deliberately a local supported-boundary guard, not a recursive validator. It does not establish generic conformance for nested objects or error DTOs; add focused evidence if those become consumed contracts. In particular, the generated 503 exception remains an error, never an admitted success value.

The generated HTTPX pool is lazy. Composition assigns the caller-owned `AsyncClient` to the observed 7.22.0 `rest_client.pool_manager` seam; both clients are closed by their context managers. Recheck this seam after generator upgrades. No handwritten fake or stub is substituted for generated production code.

## Reproduction

Save each following block at its indicated project-relative path in an isolated Python 3.12 workspace. Use JDK 25 (or a JDK compatible with the pinned generator) and OpenAPI Generator CLI **7.22.0**. The jar is a build tool, not an application dependency. Do not copy generated source into a skill or commit it. The actual executable experiment keeps `uv.lock` and ignored generated output in its temporary workspace; for adoption, resolve and retain the application's own lock.

```sh
uv sync
java -jar "$OPENAPI_GENERATOR_JAR" generate -c generator.yaml
uv run ruff check src
uv run ruff format --check src
uv run mypy src
# Actual execution requires explicitly configured real endpoints and authorization:
PYTHONPATH=src:.generated uv run python -m stock_sync.main A-1
```

These commands check the production files supplied here; they do not imply that test files are included in this reference. The separate Python testing reference documents the 34-case suite used for this example and its production contracts; it is optional reading when implementing tests, not a prerequisite for running the production snippet. The `tests` targets in the sample tool configuration apply once tests exist.

The last command performs a write: tests use controlled transports and do not run it against a live service. Set `SYNC_PROVIDER_URL`, `SYNC_INVENTORY_URL`, `SYNC_TOKEN` and optional `SYNC_TIMEOUT_SECONDS`, `SYNC_CONCURRENCY` in the environment or explicit local env file. Keep that file ignored.

The mypy override is restricted to **generated modules**: `follow_imports="silent"` retains their available annotations but suppresses checks within generator-owned code. Handwritten production **and tests** use strict mypy; there is no global `ignore_missing_imports`, no `Any` in public application contracts and no replacement stubs. Ruff also excludes generated code. The project intentionally uses `src` directly via the test path configuration; packaging/distribution metadata is not invented for this executable example.

The numeric bounds, runtime versions and atomic snapshot operation below are example-owned choices, not universal requirements of the skill.

## Files

### `pyproject.toml`

```toml
[project]
name = "stock-sync-example"
version = "0.1.0"
requires-python = ">=3.12,<3.13"
dependencies = ["httpx==0.28.1", "pydantic==2.13.5", "pydantic-settings==2.15.0", "python-dateutil==2.9.0.post0"]

[dependency-groups]
dev = ["pytest==9.1.1", "pytest-asyncio==1.4.0", "ruff==0.16.8", "mypy==2.3.1"]

[tool.pytest.ini_options]
pythonpath = ["src", ".generated"]
asyncio_mode = "auto"
testpaths = ["tests"]

[tool.ruff]
target-version = "py312"
exclude = [".generated", ".cache"]
[tool.ruff.lint]
select = ["E", "F", "I", "UP", "B", "ASYNC"]

[tool.mypy]
python_version = "3.12"
strict = true
files = ["src", "tests"]
mypy_path = "src:.generated"
plugins = ["pydantic.mypy"]
# Generated code is imported with its actual annotations, not replaced by Any.
# Its own generator defects are outside handwritten strict-check scope.
[[tool.mypy.overrides]]
module = ["inventory_client.*"]
follow_imports = "silent"

[tool.pydantic-mypy]
init_typed = true
```

### `.gitignore`

```text
.generated/
.venv/
.cache/
__pycache__/
.pytest_cache/
.mypy_cache/
.ruff_cache/
.env.local
```

### `.env.example`

Keep this safe example versioned, copy it deliberately to the ignored local file, and replace the synthetic credential before any real operation.

```dotenv
SYNC_PROVIDER_URL=https://provider.example.test/
SYNC_INVENTORY_URL=https://inventory.example.test
SYNC_TOKEN=replace-with-a-local-credential
SYNC_TIMEOUT_SECONDS=10
SYNC_CONCURRENCY=4
```

### `generator.yaml`

```yaml
generatorName: python
inputSpec: contract.yaml
outputDir: .generated
library: httpx
additionalProperties:
  packageName: inventory_client
  hideGenerationTimestamp: true
  disallowAdditionalPropertiesIfNotPresent: false
```

### `contract.yaml`

```yaml
openapi: 3.0.3
info:
  title: Inventory snapshot API
  version: 1.0.0
servers:
  - url: https://inventory.example.test
security:
  - bearerAuth: []
paths:
  /inventory-snapshots:
    post:
      operationId: create_inventory_snapshot
      tags: [Inventory]
      summary: Record a complete inventory observation
      description: Atomically records an observation; never deletes existing inventory. No automatic retry after an uncertain response.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/CreateSnapshotRequest"
      responses:
        "201":
          description: Observation recorded
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/SnapshotReceiptResponse"
        "503":
          description: Service unavailable; caller must resolve uncertain writes before retrying
          content:
            application/problem+json:
              schema:
                $ref: "#/components/schemas/ProblemDetail"
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
  schemas:
    CreateSnapshotRequest:
      type: object
      description: A complete selected set of product observations.
      additionalProperties: false
      required: [items]
      properties:
        items:
          type: array
          minItems: 1
          maxItems: 100
          description: Products explicitly requested by the caller, each present exactly once.
          items:
            $ref: "#/components/schemas/SnapshotItemRequest"
    SnapshotItemRequest:
      type: object
      description: Stock for one product at the source.
      additionalProperties: false
      required: [sku, quantity]
      properties:
        sku:
          type: string
          minLength: 1
          description: Stable provider product identifier; case preserved.
          example: A-1
        quantity:
          type: integer
          minimum: 0
          description: Available individual units, excluding reserved units.
          example: 4
    ReceiptStatusEnum:
      type: string
      description: Terminal outcome of recording this observation.
      enum: [recorded]
    SnapshotReceiptResponse:
      type: object
      description: Receipt for the atomically recorded observation; all properties required and non-null.
      additionalProperties: false
      required: [snapshotId, acceptedCount, status]
      properties:
        snapshotId:
          type: string
          minLength: 1
          description: Opaque identifier assigned by the service.
          example: snap-123
        acceptedCount:
          type: integer
          minimum: 1
          description: Number of observations committed atomically.
          example: 1
        status:
          $ref: "#/components/schemas/ReceiptStatusEnum"
    ProblemDetail:
      type: object
      description: Safe RFC 9457 error; no provider payloads or credentials.
      additionalProperties: false
      required: [type, title, status, code]
      properties:
        type:
          { type: string, format: uri, description: Stable problem type URI. }
        title: { type: string, description: Safe human-readable summary. }
        status: { type: integer, description: HTTP status code., example: 503 }
        code:
          {
            type: string,
            description: Stable machine-readable error code.,
            example: unavailable,
          }
```

### `src/stock_sync/__init__.py`

```python
"""Small typed inventory integration with explicit transport ownership."""
```

### `src/stock_sync/application.py`

```python
"""Read every requested product before recording one non-destructive snapshot."""

import asyncio
from dataclasses import dataclass
from typing import Protocol


class SourceReadError(RuntimeError):
    """Safe read failure; the chained cause remains private to diagnostic owners."""


class SnapshotWriteError(RuntimeError):
    """Write outcome is uncertain; callers must reconcile before another attempt."""


@dataclass(frozen=True)
class Observation:
    """Available units of one product, after provider validation and mapping."""

    sku: str
    quantity: int


@dataclass(frozen=True)
class SyncCommand:
    """An immutable, bounded selection; duplicate and empty identifiers are invalid."""

    skus: tuple[str, ...]

    def __post_init__(self) -> None:
        """Reject invalid selections before reading any external product."""
        if not 1 <= len(self.skus) <= 100 or len(set(self.skus)) != len(self.skus):
            raise ValueError("Choose between 1 and 100 distinct products")
        if any(not sku.strip() for sku in self.skus):
            raise ValueError("Product identifiers must not be blank")


@dataclass(frozen=True)
class SyncResult:
    """Receipt confirming how many observations were recorded atomically."""

    snapshot_id: str
    accepted_count: int


class ProductReader(Protocol):
    """Consumer boundary for a validated external product observation."""

    async def read(self, sku: str) -> Observation:
        """Return the requested product or raise SourceReadError; never invent stock."""
        ...


class SnapshotWriter(Protocol):
    """Consumer boundary for an atomic, non-destructive observation write."""

    async def record(self, items: tuple[Observation, ...]) -> SyncResult:
        """Record once or raise SnapshotWriteError; failure may follow a commit."""
        ...


class InventorySync:
    """Stateless orchestrator; collaborators own I/O, each run owns task state."""

    def __init__(
        self, reader: ProductReader, writer: SnapshotWriter, concurrency: int
    ) -> None:
        """Inject only consumed capabilities and a positive per-run read limit."""
        if concurrency < 1:
            raise ValueError("Concurrency must be positive")
        self._reader = reader
        self._writer = writer
        self._concurrency = concurrency

    async def run(self, command: SyncCommand) -> SyncResult:
        """Cancel siblings on read failure; write only after all reads succeed.

        The semaphore bounds active reads for this run. At most 100 tasks exist.
        Cancellation propagates. A cancelled write may already have committed;
        this use case does not retry or promise distributed rollback.
        """
        semaphore = asyncio.Semaphore(self._concurrency)

        async def read_one(sku: str) -> Observation:
            """Hold one concurrency slot for the complete external read."""
            async with semaphore:
                return await self._reader.read(sku)

        async with asyncio.TaskGroup() as group:
            tasks = [group.create_task(read_one(sku)) for sku in command.skus]
        return await self._writer.record(tuple(task.result() for task in tasks))
```

### `src/stock_sync/config.py`

```python
"""Startup configuration; environment overrides an explicitly selected local file."""

from pathlib import Path
from typing import Annotated

from pydantic import Field, HttpUrl, SecretStr, ValidationError
from pydantic_settings import BaseSettings, SettingsConfigDict


class ConfigurationError(ValueError):
    """Safe startup failure whose message contains no configuration values."""


class Settings(BaseSettings):
    """Validated endpoints, credentials and positive resource bounds.

    Environment strings intentionally coerce to numeric settings; external JSON
    quantities use a separate strict rule. No env file is discovered implicitly.
    """

    model_config = SettingsConfigDict(env_prefix="SYNC_", extra="forbid")
    provider_url: HttpUrl
    inventory_url: HttpUrl
    token: SecretStr = Field(min_length=1)
    timeout_seconds: Annotated[float, Field(gt=0, allow_inf_nan=False)] = 10.0
    concurrency: Annotated[int, Field(gt=0, le=20)] = 4


def load_settings(env_file: Path | None = None) -> Settings:
    """Load once before I/O; env wins over the supplied file, then defaults.

    Preserve the private cause; the CLI prints only this safe outer diagnostic.
    """
    try:
        return Settings(_env_file=env_file)
    except (ValidationError, OSError) as error:
        raise ConfigurationError("Invalid synchronization configuration") from error
```

### `src/stock_sync/adapters.py`

```python
"""Provider parsing and generated-client adaptation; DTOs stay at this boundary."""

import json
from typing import Annotated

import httpx
from inventory_client.api.inventory_api import InventoryApi
from inventory_client.exceptions import ApiException
from inventory_client.models.create_snapshot_request import CreateSnapshotRequest
from inventory_client.models.snapshot_item_request import SnapshotItemRequest
from inventory_client.models.snapshot_receipt_response import SnapshotReceiptResponse
from pydantic import BaseModel, ConfigDict, Field, StrictInt, StrictStr

from stock_sync.application import (
    Observation,
    SnapshotWriteError,
    SourceReadError,
    SyncResult,
)


class ProviderProduct(BaseModel):
    """Provider JSON: integer units required; numeric strings/bools are rejected."""

    model_config = ConfigDict(extra="forbid", frozen=True)
    sku: Annotated[StrictStr, Field(min_length=1)]
    available: Annotated[StrictInt, Field(ge=0)]


def to_observation(product: ProviderProduct) -> Observation:
    """Translate provider terminology explicitly without changing identifier case."""
    return Observation(sku=product.sku, quantity=product.available)


class HttpProductReader:
    """Reads provider JSON with a caller-owned reusable HTTPX client."""

    def __init__(self, client: httpx.AsyncClient) -> None:
        """Borrow a configured provider client; composition owns its closure."""
        self._client = client

    async def read(self, sku: str) -> Observation:
        """Raise SourceReadError for failed, malformed or mismatched product reads."""
        try:
            response = await self._client.get("products", params={"sku": sku})
            response.raise_for_status()
            product = ProviderProduct.model_validate_json(response.content)
            if product.sku != sku:
                raise ValueError("Provider returned a different product")
            return to_observation(product)
        except (httpx.HTTPError, ValueError) as error:
            # ValidationError is a ValueError; cancellation is never caught.
            raise SourceReadError("Provider product could not be read") from error


def to_request(items: tuple[Observation, ...]) -> CreateSnapshotRequest:
    """Translate only the accepted observations to generated request DTOs."""
    return CreateSnapshotRequest(
        items=[
            SnapshotItemRequest(sku=item.sku, quantity=item.quantity) for item in items
        ]
    )


async def reject_unknown_receipt_fields(response: httpx.Response) -> None:
    """Close the generator 7.22.0 extra-properties gap for this flat 201 receipt.

    Field names come from generated metadata, never a parallel schema. Other
    constraints remain on the real generated decoding path. Nested DTOs would
    require separate evidence; this hook makes no generic validation claim.
    """
    if response.status_code != 201:
        return
    await response.aread()
    payload: object = json.loads(response.content)
    allowed = {
        field.alias or name
        for name, field in SnapshotReceiptResponse.model_fields.items()
    }
    if not isinstance(payload, dict) or any(key not in allowed for key in payload):
        raise ValueError("Invalid inventory receipt shape")


class GeneratedSnapshotWriter:
    """Uses the actual generated HTTPX operation; no handwritten transport mirror."""

    def __init__(self, api: InventoryApi, timeout_seconds: float) -> None:
        """Borrow the API and apply the validated request timeout explicitly."""
        self._api = api
        self._timeout_seconds = timeout_seconds

    async def record(self, items: tuple[Observation, ...]) -> SyncResult:
        """Write once; return a receipt or raise SnapshotWriteError with its cause."""
        try:
            receipt = await self._api.create_inventory_snapshot(
                to_request(items), _request_timeout=self._timeout_seconds
            )
            if receipt.accepted_count != len(items):
                raise ValueError("Inventory receipt count differs from submitted count")
            return SyncResult(
                snapshot_id=receipt.snapshot_id, accepted_count=receipt.accepted_count
            )
        except (httpx.HTTPError, ApiException, ValueError) as error:
            raise SnapshotWriteError(
                "Inventory write could not be confirmed; reconcile before retrying"
            ) from error
```

### `src/stock_sync/main.py`

```python
"""Composition root and safe CLI error rendering; imports perform no I/O."""

import asyncio
import sys
from pathlib import Path

import httpx
from inventory_client.api.inventory_api import InventoryApi
from inventory_client.api_client import ApiClient
from inventory_client.configuration import Configuration

from stock_sync.adapters import (
    GeneratedSnapshotWriter,
    HttpProductReader,
    reject_unknown_receipt_fields,
)
from stock_sync.application import InventorySync, SyncCommand, SyncResult
from stock_sync.config import ConfigurationError, load_settings


async def execute(
    command: SyncCommand,
    env_file: Path | None = None,
    *,
    provider_transport: httpx.AsyncBaseTransport | None = None,
    inventory_transport: httpx.AsyncBaseTransport | None = None,
) -> SyncResult:
    """Validate settings first, then own both clients through success/failure/cancel.

    Transports are optional controlled test seams. Generated 7.22.0 exposes a
    lazy pool_manager; assigning the caller-owned client prevents a second pool.
    """
    settings = load_settings(env_file)
    async with (
        httpx.AsyncClient(
            base_url=str(settings.provider_url),
            timeout=settings.timeout_seconds,
            transport=provider_transport,
        ) as provider,
        httpx.AsyncClient(
            timeout=settings.timeout_seconds,
            transport=inventory_transport,
            event_hooks={"response": [reject_unknown_receipt_fields]},
        ) as inventory,
    ):
        api_client = ApiClient(
            Configuration(
                host=str(settings.inventory_url).rstrip("/"),
                access_token=settings.token.get_secret_value(),
            )
        )
        api_client.rest_client.pool_manager = inventory
        use_case = InventorySync(
            HttpProductReader(provider),
            GeneratedSnapshotWriter(InventoryApi(api_client), settings.timeout_seconds),
            settings.concurrency,
        )
        return await use_case.run(command)


def main() -> int:
    """Run explicit product identifiers; never print exception payloads or secrets."""
    try:
        result = asyncio.run(
            execute(SyncCommand(tuple(sys.argv[1:])), Path(".env.local"))
        )
    except ConfigurationError as error:
        print(str(error), file=sys.stderr)
        return 1
    except Exception:
        # Terminal boundary only: no recovery, no exception/payload logging.
        print(
            "Synchronization failed; no automatic retry was attempted", file=sys.stderr
        )
        return 1
    print(f"Recorded {result.accepted_count} observations")
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

### `src/stock_sync/csv_report.py`

```python
"""Small utility: CSV reporting needs no application service or protocol."""

import csv
from collections.abc import Iterable
from typing import TextIO

from stock_sync.application import Observation


def write_csv(items: Iterable[Observation], destination: TextIO) -> None:
    """Write a machine-readable CSV; caller owns the stream and its closure.

    csv.writer handles delimiters and quotes. This is not a spreadsheet-safe
    export for untrusted formula-like identifiers; add that policy if required.
    """
    writer = csv.writer(destination)
    writer.writerow(("sku", "quantity"))
    writer.writerows((item.sku, item.quantity) for item in items)
```

## Evidence and limits

The executable authoring check used Python 3.12.13, OpenAPI Generator 7.22.0, HTTPX 0.28.1, Pydantic 2.13.5, pydantic-settings 2.15.0, pytest 9.1.1, pytest-asyncio 1.4.0, Ruff 0.16.8, mypy 2.3.1 and JDK 25.0.4. All 34 tests passed, Ruff lint/format passed, and strict mypy passed for 7 handwritten source/test files. Two fresh generation directories produced the same 43 files byte for byte. Initial generation followed by an in-place rerun changed generator bookkeeping once; subsequent reruns were stable. This is not a claim that every future generator version has the same behavior.

Tests cover explicit mapping, strict provider values, CSV escaping, independent runs, bounded reads, sibling cancellation, failure/cancellation cleanup, invalid settings before client construction, environment precedence, safe CLI errors and preserved targeted error causes, real generated method/path/auth/body/timeout/receipt processing, malformed and partial data, uncertain write failure without retry, and the raw extra-property gap versus the guarded path. They do not verify a live inventory server's atomicity, network infrastructure, global concurrency, persistence, scheduling or idempotency. Fixed values are clearer here than Faker; rich factories are unnecessary.
