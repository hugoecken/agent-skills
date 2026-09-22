# Canonical Next.js and React tests

Use `__tests__` beside the owning role, for example `features/reservations/model/__tests__` and `features/reservations/components/__tests__`. These examples assume a real `guestLabel(count)` formatter and a controlled `ReservationForm` with `initialValues: ReservationValues` and `onSubmit(values)`; its accessible fields are Customer and Guests, and its submit button is Save. Use the actual names of the application rather than adding production APIs for the examples.

## Unit behavior and a rich factory

The form type is a named local contract because editing semantics differ from transport. If its semantics are identical to a generated DTO, use that generated type directly instead. A factory is useful when several scenarios need this complete shape; three simple values alone do not require Faker.

```tsx
import { describe, expect, it } from "vitest";
import { guestLabel } from "../guest-label";

describe("guest label", () => {
  it("uses the singular label for one guest", () => {
    // Given
    const guests = 1;

    // When
    const label = guestLabel(guests);

    // Then
    expect(label).toBe("1 guest");
  });
});
```

For a factory shared by actual form scenarios, use a separate feature-owned test-support file:

```ts
import type { Faker } from "@faker-js/faker";
import type { ReservationValues } from "../forms/reservation-values";

/** Builds a complete editing value while keeping the scenario's name and capacity explicit. */
export function validReservation(
  faker: Faker,
  customerName: string,
  guests: number,
): ReservationValues {
  return {
    customerName,
    guests,
    contactEmail: faker.internet.email(),
    note: faker.lorem.sentence(),
  };
}
```

Import the existing feature-owned factory in the tests that need it; the formatter test needs only a number. Use `new Faker({ locale: [en] })` and `faker.seed(42)` inside each consuming test. A singleton with a shared seed is unsafe across concurrent tests. Keep decisive values explicit and do not assert a particular generated email.

## Form integration

This integrates the real form component, form state and validation with browser interactions. Only the submit boundary is replaced. It does not prove HTTP or Next server semantics. The form contract preserves edits after an unrelated parent rerender.

```tsx
import { Faker, en } from "@faker-js/faker";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { expect, it, vi } from "vitest";
import { ReservationForm } from "../reservation-form";
import { validReservation } from "../../test-support/reservation-data";

it("submits the edited values after the parent rerenders", async () => {
  // Given
  const faker = new Faker({ locale: [en] });
  faker.seed(42);
  const initialValues = validReservation(faker, "Ada", 2);
  const onSubmit = vi.fn().mockResolvedValue(undefined);
  const user = userEvent.setup();
  const { rerender } = render(
    <ReservationForm initialValues={initialValues} onSubmit={onSubmit} />,
  );

  // When
  await user.clear(screen.getByRole("textbox", { name: "Customer" }));
  await user.type(screen.getByRole("textbox", { name: "Customer" }), "Grace");
  rerender(
    <ReservationForm initialValues={initialValues} onSubmit={onSubmit} />,
  );
  await user.click(screen.getByRole("button", { name: "Save" }));

  // Then
  expect(onSubmit).toHaveBeenCalledOnce();
  expect(onSubmit).toHaveBeenCalledWith({
    ...initialValues,
    customerName: "Grace",
  });
});
```

Here the several interactions form one behavior: submitting the retained edit. For a refetch regression, use the real query/form composition, a fresh QueryClient and a controlled transport response changing after editing; a parent rerender alone does not prove refetch behavior. For response-validation regressions exercise the real handwritten transport and generated schema, then assert the invalid response was not cached as success.

Use real Next/browser evidence for async server rendering, redirects and cache semantics. Keep that evidence distinct from this component integration; these examples neither add Playwright configuration nor establish a new E2E suite.

Tool mechanics: [Faker instances](https://fakerjs.dev/api/faker) and [seeding](https://fakerjs.dev/guide/usage). Use the versions supported by the owning project.
