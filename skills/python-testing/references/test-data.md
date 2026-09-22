# Targeted typed test data

Use this pattern only when several tests share a rich valid object. The `Customer` below is an illustrative application value, not a transport DTO or a mandatory model. The decisive identifier and activity stay explicit; Faker supplies only the unimportant display name and contact email. These addresses are synthetic test data.

```python
"""Application-owned customer value and a focused example test factory."""

from dataclasses import dataclass
from uuid import UUID

from faker import Faker


@dataclass(frozen=True)
class Customer:
    """Customer projection whose activity controls eligibility."""

    id: UUID
    display_name: str
    contact_email: str
    active: bool


def valid_customer(fake: Faker, customer_id: UUID, *, active: bool) -> Customer:
    """Fill secondary fields without hiding scenario identity or eligibility."""
    return Customer(
        id=customer_id,
        display_name=fake.name(),
        contact_email=fake.email(),
        active=active,
    )


def test_factory_preserves_scenario_fields() -> None:
    # Given
    fake = Faker("en_US")
    fake.seed_instance(42)
    customer_id = UUID("c17c5c0d-9134-4d55-81ef-472fa2b4be47")

    # When
    customer = valid_customer(fake, customer_id, active=False)

    # Then
    assert customer.id == customer_id
    assert customer.active is False
    assert customer.display_name
    assert customer.contact_email
```

In a project, the value belongs to its application owner and the factory stays near its consuming tests. This focused factory test demonstrates its explicit overrides; application behavior tests must still assert the real eligibility or other outcome they protect. Do not add factory tests mechanically to every constructor helper.

Use a new local seeded instance per test or Faker's standard pytest fixture. Do not share mutable random state across tests, assert exact generated strings, generate object graphs by reflection or add a replay framework. The locale and library version belong to the project when they affect a scenario. Small records in the [integration example](examples.md) use literals because a fake-data dependency adds no value there.
