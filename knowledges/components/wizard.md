# Component Guide: Wizard / Stepper (`WizardComponent`)

`WizardComponent` is a multi-step form container in Skalfa App. It is used to break down complex forms into a sequential, step-by-step process.

---

## 1. Component Interface (`WizardProps`)

```typescript
export interface WizardProps {
  steps: {
    title    : string;               // Title of the step
    content  : ReactNode;             // Form or content rendered in this step
  }[];
  activeStep ?: number;               // Index of the current active step (0-indexed)
  onStepChange?: (step: number) => void;// Callback when step changes
  onComplete  ?: () => void;          // Callback when the last step is submitted/completed
  className  ?: string;
}
```

---

## 2. Key Features

*   **Step Indicator**: Displays a top progress bar with numbered circles indicating the completed, active, and upcoming steps.
*   **Navigation Buttons**: Automatically renders "Back" and "Next / Complete" buttons at the bottom.
*   **Validation Guard**: You can control the `activeStep` externally to prevent the user from advancing to the next step until the current step's form validation passes.

---

## 3. Usage Example

```tsx
import { WizardComponent } from "@components";
import { useState } from "react";

export function MultiStepRegistration() {
  const [currentStep, setCurrentStep] = useState(0);

  const steps = [
    {
      title: "Account Details",
      content: (
        <div className="space-y-4">
          <p>Please enter your email and password.</p>
          {/* Input fields */}
        </div>
      )
    },
    {
      title: "Personal Profile",
      content: (
        <div className="space-y-4">
          <p>Please enter your full name and address.</p>
          {/* Input fields */}
        </div>
      )
    },
    {
      title: "Confirmation",
      content: (
        <div className="space-y-4">
          <p>Review your details before submitting.</p>
        </div>
      )
    }
  ];

  const handleComplete = () => {
    alert("Registration completed!");
  };

  return (
    <div className="max-w-xl mx-auto p-6 bg-white border rounded-lg shadow-sm">
      <WizardComponent
        steps={steps}
        activeStep={currentStep}
        onStepChange={setCurrentStep}
        onComplete={handleComplete}
      />
    </div>
  );
}
```
