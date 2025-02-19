## Adhering to a Design Language

### 1. **Define a Tailwind Config Based on Your Company’s Design Tokens**

- Extend **colors, spacing, typography**, and other styles in `tailwind.config.ts`:

```ts
import { type Config } from "tailwindcss";

const config: Config = {
  theme: {
    extend: {
      colors: {
        primary: "#123456", // Replace with your brand color
        secondary: "#654321",
        accent: "#ffcc00",
      },
      fontFamily: {
        sans: ["YourCompanyFont", "sans-serif"],
      },
    },
  },
};

export default config;
```

### 2. **Customize Shadcn Components to Match Your Design**

- Override styles in `components/ui/`:

```tsx
// components/ui/button.tsx
import { Button as ShadButton, type ButtonProps } from "shadcn/ui/button";
import { cn } from "@/lib/utils"; // Utility to merge class names

export function Button({ className, ...props }: ButtonProps) {
  return (
    <ShadButton
      className={cn(
        "bg-primary text-white rounded-lg shadow-md px-4 py-2",
        className
      )}
      {...props}
    />
  );
}
```

- Repeat this process for other Shadcn components like **card, input, dialog**, etc.

### 3. **Use Global Styles for Further Customization**

- Modify `globals.css` or `app/globals.css`:

```css
@layer base {
  body {
    @apply bg-gray-50 text-gray-900 font-sans;
  }

  h1 {
    @apply text-3xl font-bold text-primary;
  }
}
```

### 4. **Ensure Consistency With a Theme Provider**

- If your company has a **theme system**, wrap your app in a `ThemeProvider`:

```tsx
import { ThemeProvider } from "next-themes";

export function Providers({ children }: { children: React.ReactNode }) {
  return <ThemeProvider>{children}</ThemeProvider>;
}
```

### 5. **Create a Design System Doc for Developers**

- Document **color usage, spacing guidelines, typography, and component variations**.
- Consider **Storybook** for visual consistency.

This approach ensures that **every component automatically adheres to your company’s design system** while keeping customization **scalable and maintainable**. 🚀
