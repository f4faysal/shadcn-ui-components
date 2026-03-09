# GitHub Copilot Instructions

This repository contains beautiful, reusable UI components built with [shadcn/ui](https://ui.shadcn.com/) and [Tailwind CSS](https://tailwindcss.com/). Use the following guidelines when generating or suggesting code.

## Project Overview

- **Framework**: React (with TypeScript)
- **Component Library**: shadcn/ui (built on Radix UI primitives)
- **Styling**: Tailwind CSS with `cn()` utility from `clsx` + `tailwind-merge`
- **Goal**: Clean, accessible, reusable, and well-documented UI components for the open-source community

---

## Component Structure

Every component should follow this file structure:

```
src/
  components/
    ui/                  # shadcn/ui base components (auto-generated, do not modify)
    <ComponentName>/
      index.tsx          # Main component export
      <ComponentName>.tsx
      <ComponentName>.stories.tsx   # Storybook stories (if applicable)
      README.md          # Usage documentation
```

### Component Template

When generating a new component, use this structure:

```tsx
import * as React from "react"
import { cn } from "@/lib/utils"

export interface <ComponentName>Props
  extends React.HTMLAttributes<HTMLDivElement> {
  // define props here
}

const <ComponentName> = React.forwardRef<HTMLDivElement, <ComponentName>Props>(
  ({ className, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          // base Tailwind classes here
          className
        )}
        {...props}
      />
    )
  }
)
<ComponentName>.displayName = "<ComponentName>"

export { <ComponentName> }
```

---

## Coding Conventions

- **Always use TypeScript** with explicit prop interface definitions.
- **Use `React.forwardRef`** so consumers can attach refs to DOM elements.
- **Export a named export** (not default) for every component.
- **Use the `cn()` utility** (from `@/lib/utils`) to merge Tailwind classes and allow className overrides.
- **Prefer composition** over complex prop drilling; expose sub-components where appropriate (e.g., `Card`, `CardHeader`, `CardContent`).
- **Keep components headless-friendly**: separate logic from styling where possible.
- **Avoid inline styles**; use Tailwind utility classes exclusively.
- **Use semantic HTML elements** (e.g., `<button>`, `<nav>`, `<section>`) for correct semantics.

---

## Accessibility (a11y)

All components must be accessible by default:

- Use Radix UI primitives (via shadcn/ui) for interactive components (Dialog, Dropdown, Tooltip, etc.) as they handle ARIA attributes and keyboard navigation automatically.
- Always provide `aria-label` or `aria-labelledby` for icon-only buttons and controls.
- Ensure sufficient color contrast ratios (WCAG AA minimum).
- Support keyboard navigation: `Tab`, `Enter`, `Space`, `Escape`, and arrow keys where applicable.
- Use `role` attributes only when a semantic HTML element is not available.
- Never remove focus outlines; use `focus-visible:ring-*` Tailwind classes instead.

```tsx
// Good - accessible icon button
<button
  aria-label="Close dialog"
  className="focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring"
>
  <XIcon className="h-4 w-4" />
</button>
```

---

## Tailwind CSS Guidelines

- Use the design tokens defined in `tailwind.config.ts` (colors, spacing, border-radius, etc.).
- Prefer semantic color variables (`bg-background`, `text-foreground`, `border-border`) over hardcoded colors so the component works in both light and dark modes.
- Use responsive prefixes (`sm:`, `md:`, `lg:`) for adaptive layouts.
- Use `group` and `peer` utilities for compound component interactions.
- Extract repeated class combinations into a `cva` (class-variance-authority) variant definition when a component has multiple variants:

```tsx
import { cva, type VariantProps } from "class-variance-authority"

const buttonVariants = cva(
  "inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        outline: "border border-input bg-background hover:bg-accent hover:text-accent-foreground",
        ghost: "hover:bg-accent hover:text-accent-foreground",
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-11 rounded-md px-8",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
)
```

---

## Documentation

Every component should include:

1. **JSDoc comment** on the component and its props:

```tsx
/**
 * A versatile card component for displaying grouped content.
 *
 * @example
 * <Card>
 *   <CardHeader>
 *     <CardTitle>Title</CardTitle>
 *   </CardHeader>
 *   <CardContent>Content goes here.</CardContent>
 * </Card>
 */
```

2. **README.md** inside the component folder with:
   - Short description
   - Installation / import instructions
   - Props table (name, type, default, description)
   - Usage examples (copy-pasteable code snippets)

---

## Design Patterns

- **Compound Components**: expose sub-components (e.g., `<Select>`, `<SelectTrigger>`, `<SelectContent>`, `<SelectItem>`) to give consumers full control.
- **Render Props / Slots**: prefer Radix UI's `asChild` pattern to allow consumers to change the underlying element.
- **Controlled & Uncontrolled**: support both modes for form-related components using React's controlled/uncontrolled patterns.
- **Polymorphic Components**: when an element type needs to be configurable, use the `asChild` prop from Radix UI instead of a custom `as` prop.

---

## Dependencies

Core dependencies used in this project:

| Package | Purpose |
|---|---|
| `react` / `react-dom` | UI framework |
| `tailwindcss` | Utility-first CSS |
| `clsx` + `tailwind-merge` | Class merging (`cn()` helper) |
| `class-variance-authority` | Component variant management |
| `@radix-ui/*` | Accessible headless primitives |
| `lucide-react` | Icon library |

When suggesting new dependencies, check for compatibility with the packages listed above and prefer packages that are already in the project before adding new ones.

---

## Code Quality

- Write components that are **pure and side-effect free** where possible.
- Avoid `any` types; use proper TypeScript generics or specific types.
- Keep components **small and focused** (single responsibility).
- Use **named exports** to improve tree-shaking.
- Ensure components are **testable** by keeping logic separate from rendering.
