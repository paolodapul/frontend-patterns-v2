### Encapsulating Tailwind Classes Inside Components

This approach is best when:

- You want a consistent design system.
- Your components will be reused often across the project.
- You don't want to repeat Tailwind classes in multiple places.

**Example:**

```tsx
import { cn } from "@/lib/utils"; // Shadcn utility for class merging

type ButtonProps = {
  variant?: "primary" | "secondary";
} & React.ButtonHTMLAttributes<HTMLButtonElement>;

export function Button({
  variant = "primary",
  className,
  ...props
}: ButtonProps) {
  return (
    <button
      className={cn(
        "px-4 py-2 rounded-md text-white transition",
        variant === "primary"
          ? "bg-blue-500 hover:bg-blue-600"
          : "bg-gray-500 hover:bg-gray-600",
        className // Allows for optional customization
      )}
      {...props}
    />
  );
}
```

Usage:

```tsx
<Button onClick={handleClick}>Click Me</Button>
<Button variant="secondary">Secondary</Button>
```
