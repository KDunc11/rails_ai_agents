---
paths:
  - "app/views/**/*.erb"
  - "app/components/**/*.rb"
  - "app/components/**/*.erb"
  - "spec/components/**/*.rb"
  - "app/decorators/**/*.rb"
  - "spec/decorators/**/*.rb"
---

# View & Component Conventions

- Use ViewComponents (`app/components/`) for reusable UI elements over partials
- Use decorators (`app/decorators/`) with Draper Decorators for formatting logic
- No business logic in views -- use decorators for display formatting
- Turbo Frames for partial page updates; Turbo Streams for multi-target updates
- Stimulus controllers for client-side behavior (minimal JS, progressive enhancement)
- Tailwind CSS 4 utility classes for styling
- Always include ARIA attributes for accessibility (WCAG 2.1 AA)
