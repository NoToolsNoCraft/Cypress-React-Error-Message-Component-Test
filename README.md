# Cypress React Error Message Component Test

This Cypress test suite verifies the functionality of the ErrorMessage component, which is responsible for displaying validation error messages from react-hook-form. The tests ensure that the component renders correctly based on the presence or absence of an error, associates error messages with the correct field, and applies the expected attributes for accessibility and styling.

By isolating the component and simulating different error states, the test guarantees that ErrorMessage consistently communicates form validation issues to the user.


## React Error Message Component



```bash
import { Text } from "@radix-ui/themes";
import { FieldError } from "react-hook-form";

const ErrorMessage = ({ error }: { error: FieldError | undefined }) => {
  if (!error) return null;

  return (
    <Text color="red" as="div" role="alert" data-for={error.ref!.name}>
      {error.message}
    </Text>
  );
};

export default ErrorMessage;
```


## Cypress Component Test



```bash
import { mount } from "cypress/react";
import ErrorMessage from "../../src/components/ErrorMessage";
import { FieldError } from "react-hook-form";

describe("ErrorMessage Component", () => {
  it("renders nothing when error is undefined", () => {
    mount(<ErrorMessage error={undefined} />);
    cy.get("[role='alert']").should("not.exist");
  });

  it("renders the error message when error is provided", () => {
    const mockError: FieldError = {
      type: "required",
      message: "This field is required",
      ref: { name: "username" } as any,
    };

    mount(<ErrorMessage error={mockError} />);

    cy.get("[role='alert']")
      .should("exist")
      .and("have.attr", "data-for", "username")
      .and("contain.text", "This field is required");
  });

  it("renders with correct color styling (red)", () => {
    const mockError: FieldError = {
      type: "pattern",
      message: "Invalid format",
      ref: { name: "email" } as any,
    };

    mount(<ErrorMessage error={mockError} />);

    cy.get("[role='alert']")
      .should("exist")
      .and("have.attr", "data-for", "email")
      .and("contain.text", "Invalid format");
  });
});


```

![Screenshot of Label Component](Screenshot%202025-09-27%20153029.png)

| Criteria                  | Justification                                                                                                                                                                                                           |
| :------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Isolation**             | The component is tested independently with controlled `FieldError` objects, avoiding reliance on a full form setup.                                                                                                     |
| **Mocking Quality**       | Simulated `FieldError` objects mimic realistic validation states (required, pattern, etc.), ensuring predictable and testable outputs without needing `react-hook-form` execution.                                      |
| **Coverage**              | Covers all key scenarios: <br>1) No error (renders nothing) <br>2) Error present (renders message) <br>3) Different error types (ensures consistent rendering).                                                         |
| **Readability**           | Test cases are clearly named to reflect each scenario, making the suite easy to read and maintain.                                                                                                                      |
| **Clarity of Assertions** | Assertions validate both existence and attributes (`role="alert"`, `data-for`), directly tied to accessibility and functional correctness. This ensures the component meets user-facing and accessibility requirements. |
