# Canonical Python tests

Mirror production owners, for example `tests/reservations/domain/test_capacity.py` and `tests/reservations/infrastructure/test_provider.py`. The small production examples below make the boundary explicit; they are not a proposed framework. Application commands and generated DTOs keep their actual owners.

## Pure behavior under test

```python
"""Capacity validation independent of transport or persistence."""


def available_places(capacity: int, reserved: int) -> int:
    """Return remaining places, rejecting invalid capacity or reservation counts."""
    if capacity < 0 or reserved < 0 or reserved > capacity:
        raise ValueError("Invalid capacity")
    return capacity - reserved
```

## Unit cases

```python
"""Observable capacity boundaries."""

import pytest

from example.reservations.domain.capacity import available_places


@pytest.mark.parametrize(("capacity", "reserved", "remaining"), [(4, 0, 4), (4, 3, 1), (4, 4, 0)])
def test_returns_remaining_places(capacity: int, reserved: int, remaining: int) -> None:
    # Given
    expected = remaining

    # When
    result = available_places(capacity, reserved)

    # Then
    assert result == expected


def test_rejects_reservations_above_capacity() -> None:
    # Given
    capacity = 4
    reserved = 5

    # When / Then: the exception assertion encloses the operation.
    with pytest.raises(ValueError, match="Invalid capacity"):
        available_places(capacity, reserved)
```

For exception tests, an explicit combined `When / Then` is preferable to capturing an exception in a custom helper merely to separate comments. Java's deferred throwing callable and JavaScript's `expect(() => ...)` likewise keep the operation visible.

## Rich data without hidden setup

For an owned internal `Customer` dataclass with ID, display name, contact email and active status, put this factory near the tests that share it. A UUID identifies an explicit scenario; Faker supplies only non-decisive values. No database insertion or mock installation happens here.

```python
"""Shared customer data for reservation tests."""

from uuid import UUID

from faker import Faker

from example.reservations.domain.customer import Customer


def valid_customer(faker: Faker, customer_id: UUID, *, active: bool) -> Customer:
    """Build secondary customer fields while keeping identity and activity explicit."""
    return Customer(
        id=customer_id,
        display_name=faker.name(),
        contact_email=faker.email(),
        active=active,
    )
```

Use Faker's built-in pytest `faker` fixture, reseeded per test by its standard plugin, or an explicitly local `Faker()` instance with `seed_instance(42)`. Do not add a custom replay fixture. Fix locale where a locale-specific format is part of the scenario. Do not assert a particular generated string. Internal models do not become Pydantic models merely to use this factory.

## HTTP integration

Suppose the actual provider adapter has `async fetch_customer(client: httpx.AsyncClient, customer_id: UUID) -> ProviderCustomer`, calls `/customers/{id}` and translates a timeout into `ProviderUnavailable`. This test runs the real adapter and HTTPX request/exception path with a controlled transport. It does not prove network socket behavior or generated OpenAPI validation. The project already supplies the pytest async plugin used by this example.

```python
"""Provider adapter behavior through a controlled HTTPX transport."""

from uuid import UUID

import httpx
import pytest

from example.reservations.infrastructure.provider import ProviderUnavailable, fetch_customer


@pytest.mark.asyncio
async def test_translates_provider_timeout_without_returning_empty_data() -> None:
    # Given
    customer_id = UUID("00000000-0000-0000-0000-000000000001")

    def timeout(request: httpx.Request) -> httpx.Response:
        """Fail the expected operation at the transport boundary."""
        assert request.method == "GET"
        assert request.url.path == f"/customers/{customer_id}"
        raise httpx.ReadTimeout("Controlled timeout", request=request)

    transport = httpx.MockTransport(timeout)
    async with httpx.AsyncClient(transport=transport, base_url="https://provider.invalid") as client:
        # When / Then
        with pytest.raises(ProviderUnavailable):
            await fetch_customer(client, customer_id)
```

When the changed adapter owns writes, separately prove that a failed/partial read produces no replacement/deletion. When generated decoding changes, exercise the real generated client and its actual validation path with malformed responses; a handwritten stand-in model is not that evidence. Keep resources scoped and closed. Reuse the repository's async test mode rather than adding a second plugin.

Tool mechanics: [Faker pytest fixtures](https://faker.readthedocs.io/en/master/pytest-fixtures.html) and [HTTPX transports](https://www.python-httpx.org/advanced/transports/). Use the versions supported by the owning project.
