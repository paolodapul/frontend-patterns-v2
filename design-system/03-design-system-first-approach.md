## Design System-First Workflow

### **1. Core Design System: Encapsulated Components**

- Components should **encapsulate Tailwind classes** to enforce consistency.
- Provide **variants** using `clsx` or `cn` (from Shadcn) to cover common styles.
- Allow `className` as an **escape hatch** for overriding styles.

**Example: Button Component**

```tsx
import { cn } from "@/lib/utils";

type ButtonProps = {
  variant?: "primary" | "secondary" | "danger";
  size?: "sm" | "md" | "lg";
} & React.ButtonHTMLAttributes<HTMLButtonElement>;

export function Button({
  variant = "primary",
  size = "md",
  className,
  ...props
}: ButtonProps) {
  return (
    <button
      className={cn(
        "rounded-md font-medium transition focus:outline-none focus:ring-2",
        {
          primary: "bg-blue-500 text-white hover:bg-blue-600",
          secondary: "bg-gray-500 text-white hover:bg-gray-600",
          danger: "bg-red-500 text-white hover:bg-red-600",
        }[variant],
        {
          sm: "px-2 py-1 text-sm",
          md: "px-4 py-2 text-base",
          lg: "px-6 py-3 text-lg",
        }[size],
        className // Escape hatch
      )}
      {...props}
    />
  );
}
```

**✅ Enforces styles but allows modifications via `className`.**

---

### **2. Customization via Utility Components**

If styles deviate too much, provide **utility components** where devs can pass raw Tailwind classes.

**Example: Styled Box Component**

```tsx
type BoxProps = {
  as?: keyof JSX.IntrinsicElements;
  className?: string;
  children: React.ReactNode;
};

export function Box({ as: Tag = "div", className, children }: BoxProps) {
  return <Tag className={cn("p-4 border rounded", className)}>{children}</Tag>;
}
```

Usage:

```tsx
<Box className="bg-purple-500 text-white">Custom Box</Box>
```

**✅ Gives flexibility but still applies default styles.**

---

### **3. Enforce Usage with ESLint & Documentation**

- Use **ESLint rules** to prevent direct Tailwind usage in pages/layouts.
- Document component usage in a **Storybook** or a simple MDX guide.

Example ESLint rule:

```json
"no-restricted-imports": [
  "error",
  {
    "patterns": ["tailwindcss"],
    "message": "Use design system components instead of raw Tailwind classes."
  }
]
```

---

### **4. Escape Hatch: Special Styling Zones**

For rare cases where inline styles or `style={{}}` are needed:

- Provide an `Unstyled` component.
- Use `tailwind-merge` to combine base and custom styles safely.

**Example:**

```tsx
import { twMerge } from "tailwind-merge";

export function UnstyledDiv({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) {
  return <div className={twMerge(className)} {...props} />;
}
```

Usage:

```tsx
<UnstyledDiv className="absolute top-0 left-0 w-full h-full bg-black/50" />
```

**✅ Keeps the main system clean while allowing escape hatches.**

---

### **Final Project Structure**

```
/components
  /ui
    button.tsx  // Design system component
    card.tsx
    box.tsx
  /unstyled
    unstyled-div.tsx  // Escape hatch

/lib
  utils.ts   // cn, tailwind-merge

/pages
  index.tsx  // Uses design system components only
```

---

### **Conclusion**

1. **Encapsulate styles in components** and offer controlled variants.
2. **Allow `className` as an escape hatch** while keeping core styles.
3. **Use utility components for flexible layouts.**
4. **Enforce best practices via ESLint and documentation.**
5. **Provide a last-resort `Unstyled` component** for extreme cases.
