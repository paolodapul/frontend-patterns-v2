## Addressing Design Gaps and Overuse of Custom Styles

If developers are resorting to raw Tailwind classes because the standard components don’t match the UI designs, the project team should take a **systematic approach to improve the design system** rather than enforcing strict rules that might slow down development. Here's what the team should do:

---

### **1. Conduct a Design Gap Analysis**

- Identify **which components** are insufficient for the current UI designs.
- Determine **patterns** where developers repeatedly override styles.
- Group these **common overrides** and assess whether they should be part of the design system.

**Action Step:**  
Hold a **bi-weekly design review meeting** to analyze where developers are using raw Tailwind classes instead of reusable components.

---

### **2. Introduce More Component Variants**

- If developers keep overriding styles, **expand the component API** to include those use cases.
- Use **variants and size props** in components to provide more flexibility.

**Example: Expanding the Button Component**
Before (Limited Styles):

```tsx
export function Button({
  variant = "primary",
  className,
  ...props
}: ButtonProps) {
  return <button className={cn("bg-blue-500", className)} {...props} />;
}
```

After (More Variants):

```tsx
export function Button({
  variant = "primary",
  rounded = false,
  className,
  ...props
}: ButtonProps) {
  return (
    <button
      className={cn(
        "px-4 py-2 transition focus:ring-2",
        variant === "primary"
          ? "bg-blue-500 text-white hover:bg-blue-600"
          : "bg-gray-500",
        rounded ? "rounded-full" : "rounded-md",
        className
      )}
      {...props}
    />
  );
}
```

✅ **Developers no longer need to override styles manually.**

---

### **3. Create a Developer Feedback Process**

If UI changes are frequent, create a **formal request process** where developers can:

- Request new component variants.
- Submit a proposal for updates to the design system.

**Action Step:**  
Use a **GitHub discussion board or Notion page** for developers to propose design system improvements.

---

### **4. Allow Utility Components for Edge Cases**

If designs are **one-off or experimental**, allow a **utility component** for temporary solutions.

**Example: CustomContainer Component**

```tsx
export function CustomContainer({
  className,
  children,
}: {
  className?: string;
  children: React.ReactNode;
}) {
  return <div className={cn("p-4 border", className)}>{children}</div>;
}
```

Usage:

```tsx
<CustomContainer className="bg-red-500 shadow-lg">
  Experimental UI
</CustomContainer>
```

✅ **Prevents raw Tailwind usage while allowing flexibility.**

---

### **5. Enforce Design System Usage with Linting (With Escape Hatches)**

Set up **ESLint rules** to prevent raw Tailwind usage in pages/layouts but allow exceptions.

Example ESLint Rule:

```json
"no-restricted-syntax": [
  "error",
  {
    "selector": "JSXAttribute[name.name='className'][value.value=/\b(bg-|text-|p-|m-)\b/]",
    "message": "Use design system components instead of raw Tailwind classes."
  }
]
```

✅ **Developers are reminded to use the design system, but they can still override when necessary.**

---

### **6. Maintain an "Experimental Components" Library**

For frequently used but non-standard designs, maintain a **separate folder** for components that might later become official.

```
/components
  /ui        (Stable components)
  /experimental  (New designs that may become standard)
```

Developers can use **experimental components** instead of writing raw Tailwind.

✅ **Gives developers flexibility without breaking the system.**

---

### **Final Action Plan for the Team**

✅ **Short-Term Fixes:**

1. **Identify gaps** in the design system where developers frequently use raw Tailwind.
2. **Introduce new variants** in reusable components.
3. **Allow controlled overrides** via `className` but track usage.

✅ **Long-Term Strategy:** 4. **Create a request process** for design system updates. 5. **Use linting rules** to encourage design system usage. 6. **Maintain an experimental components library** for evolving designs.
