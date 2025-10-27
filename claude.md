# NBA Card Game - Claude Development Guide

## Tech Stack

### Core Technologies
- **Runtime**: Bun (with hot module replacement)
- **Framework**: React 19
- **Language**: TypeScript (strict mode)
- **Styling**: Tailwind CSS 4
- **Component Library**: shadcn/ui (New York style)
- **Build Tool**: Custom Bun build script with Tailwind plugin

### Key Dependencies
- **UI Primitives**: Radix UI (@radix-ui/react-*)
- **Styling Utilities**:
  - `class-variance-authority` (cva) for component variants
  - `tailwind-merge` + `clsx` via `cn()` utility
- **Icons**: Lucide React
- **Animations**: tw-animate-css

## Code Style & Patterns

### Component Structure
- Use functional components with TypeScript
- Prefer named exports for components
- Use `React.ComponentProps` for extending native elements
- Leverage Radix UI's `Slot` for polymorphic components (`asChild` pattern)

### Styling Guidelines
1. **Always use shadcn/ui components** when available before creating custom components
2. **Use Tailwind utilities** for all styling (avoid custom CSS unless necessary)
3. **Use the `cn()` utility** from `@/lib/utils` to merge class names
4. **Component variants**: Use `class-variance-authority` (cva) for component variations
5. **CSS Variables**: shadcn uses CSS variables for theming - respect this pattern
6. **Dark mode support**: Ensure components support dark mode via CSS variables

### Import Aliases
Use TypeScript path aliases for cleaner imports:
```typescript
import { Button } from "@/components/ui/button"
import { cn } from "@/lib/utils"
import type { MyType } from "@/lib/types"
```

Available aliases:
- `@/components` - React components
- `@/components/ui` - shadcn UI components
- `@/lib` - Utilities and helpers
- `@/hooks` - Custom React hooks

### TypeScript Configuration
- **Strict mode enabled** - all strict checks are on
- **Module Resolution**: "bundler" mode (Bun-specific)
- **JSX**: React 19 automatic runtime (`react-jsx`)
- **Notable settings**:
  - `noUncheckedIndexedAccess: true` - always check array access
  - `noImplicitOverride: true` - explicit override keyword
  - `verbatimModuleSyntax: true` - type imports must use `type` keyword

### File Organization
```
src/
├── components/
│   └── ui/           # shadcn components (button, card, input, etc.)
├── lib/
│   └── utils.ts      # cn() and other utilities
├── App.tsx           # Main app component
├── index.ts          # Bun server entry point
└── index.css         # Global styles and Tailwind imports
```

## Development Workflow

### Running the App
```bash
bun dev          # Development server with hot reload
bun start        # Production server
bun run build    # Build for production
```

### Adding shadcn Components
When you need a new UI component, check shadcn/ui first:
```bash
bunx shadcn@latest add <component-name>
```

Components are configured to use:
- Style: New York
- Base color: Neutral
- CSS variables for theming
- Lucide icons

### Server-Side Routes
The app uses Bun's native server with route definitions in `src/index.ts`:
- Define routes in the `routes` object
- Support for multiple HTTP methods (GET, PUT, etc.)
- Dynamic route parameters (e.g., `/api/hello/:name`)
- HTML files are served via `/*` catch-all route

## Best Practices

### Component Patterns
1. **Button-like components**: Use `asChild` prop with Radix Slot for composition
2. **Form elements**: Extend native HTML props with `React.ComponentProps<"element">`
3. **Variants**: Define using cva with TypeScript inference via `VariantProps`

Example:
```typescript
const buttonVariants = cva(
  "base-classes",
  {
    variants: {
      variant: { default: "...", destructive: "..." },
      size: { default: "...", sm: "...", lg: "..." }
    },
    defaultVariants: { variant: "default", size: "default" }
  }
)

function Button({ variant, size, asChild, ...props }:
  React.ComponentProps<"button"> & VariantProps<typeof buttonVariants> & {
    asChild?: boolean
  }) {
  const Comp = asChild ? Slot : "button"
  return <Comp className={cn(buttonVariants({ variant, size }))} {...props} />
}
```

### Styling Patterns
1. Use Tailwind utility classes directly in JSX
2. Use `cn()` to conditionally merge classes
3. Prefer Tailwind's built-in responsive/state variants over custom CSS
4. Use `[&_selector]:utility` syntax for complex CSS selectors when needed
5. Respect the design system's spacing, colors, and typography scales

### Type Safety
1. Always type component props explicitly
2. Use `type` keyword for type-only imports
3. Leverage TypeScript's utility types (`React.ComponentProps`, `VariantProps`, etc.)
4. Avoid `any` - use `unknown` or proper types

### Performance
1. Use React 19 features appropriately
2. Leverage Bun's hot reload during development
3. Build script minifies and creates source maps for production
4. Code splitting handled by Bun's build system

## Common Patterns

### Adding a New Feature Component
1. Check if shadcn has a relevant UI component
2. Create component in `src/components/`
3. Use TypeScript with proper typing
4. Import UI components from `@/components/ui/`
5. Use `cn()` for dynamic class merging
6. Export as named export

### Adding Server Routes
1. Add route definition in `src/index.ts` routes object
2. Use async handlers that return `Response.json()`
3. Access params via `req.params`
4. Support multiple HTTP methods when needed

### Styling a Component
1. Start with shadcn component if available
2. Use Tailwind utilities for spacing, colors, typography
3. Use cva for component variants
4. Use `cn()` to merge conditional classes
5. Ensure dark mode support via CSS variables

## Documentation References
- [Bun Documentation](https://bun.sh/docs)
- [React 19 Documentation](https://react.dev)
- [shadcn/ui](https://ui.shadcn.com)
- [Tailwind CSS 4](https://tailwindcss.com/docs)
- [Radix UI](https://www.radix-ui.com)
- [Lucide Icons](https://lucide.dev)
