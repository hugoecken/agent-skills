# Canonical Next.js and React tests

Use `__tests__` beside the owning role, for example `features/reservations/model/__tests__` and `features/reservations/components/__tests__`. These examples assume a real `guestLabel(count)` formatter and a controlled `ReservationForm` with `initialValues: ReservationValues` and `onSubmit(values)`; its accessible fields are Customer and Guests, and its submit button is Save. Use the actual names of the application rather than adding production APIs for the examples.

## Unit behavior and a rich factory

The form type is a named local contract because editing semantics differ from transport. Reuse an existing type when its semantics match. A factory is useful when several scenarios need the same complete shape.

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
import type { ReservationValues } from "../forms/reservation-values";

/** Builds a complete editing value while keeping the scenario's name and capacity explicit. */
export function validReservation(
  customerName: string,
  guests: number,
): ReservationValues {
  return {
    customerName,
    guests,
    contactEmail: "guest@example.invalid",
    note: "Window table",
  };
}
```

Import the feature-owned factory in the tests that need a complete form value. The formatter test needs only a number.

## Form integration

This integrates the real form component, form state and validation with browser interactions. Only the submit boundary is replaced. It does not prove HTTP or Next server semantics. The form contract preserves edits after an unrelated parent rerender.

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { expect, it, vi } from "vitest";
import { ReservationForm } from "../reservation-form";
import { validReservation } from "../../test-support/reservation-data";

it("submits the edited values after the parent rerenders", async () => {
  // Given
  const initialValues = validReservation("Ada", 2);
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

These interactions verify that the form submits the retained edit. For a refetch regression, render the form with its real data layer and control the transport so the next response changes after editing. To test response validation, send invalid data through the feature's actual response handler and assert the failure the feature exposes. If the feature caches responses, isolate its cache and verify that the invalid data never becomes a successful entry.

Use real Next/browser evidence for async server rendering, redirects and cache semantics. Keep that evidence distinct from this component integration; these examples neither add Playwright configuration nor establish a new E2E suite.
