### Allowing Tailwind Classes to be Passed via Props

This approach is best when:

- You need flexibility for different use cases.
- The design system isn’t rigid.
- You want to override styles per instance.

Example:

```tsx
<Button className="bg-red-500 hover:bg-red-600">Custom Button</Button>
```

This allows modifications at the page level.
