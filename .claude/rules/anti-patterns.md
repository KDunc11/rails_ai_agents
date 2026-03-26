---
paths:
  - "app/**/*.rb"
  - "spec/**/*.rb"
---

# Anti-Patterns to Avoid

- **God Model**: If a model exceeds ~200 lines or handles auth, billing, notifications, and reporting -- extract business logic to focused service objects. The model keeps only persistence, validations, associations, and simple scopes.
- **Service Graveyard**: Don't create services for trivial CRUD. `user.update!(name: params[:name])` is fine inline. The bar for extraction is real complexity -- multi-step operations, external calls, or cross-model orchestration.
- **Callback Spaghetti**: Never use `after_create`/`after_save` for sending emails, enqueuing jobs, calling external APIs, or creating related records with business logic. These are contextual side effects that belong in explicit service calls.
- **Premature Abstraction**: Don't create a base class for two services. Don't create a helper for one view. Don't design for hypothetical future requirements. The right amount of complexity is the minimum needed now.
- **Over-Testing**: Test behavior, not implementation. Don't test framework behavior (Rails validations work). Don't test private methods (test via public interface). Don't assert which internal method was called.
- **STI Abuse**: If more than 20% of columns are subtype-specific (lots of NULLs), use polymorphic associations with separate tables instead of Single Table Inheritance.
- **N+1 Ignorance**: Always eager-load associations you know you'll access (`includes`, `preload`). Use `strict_loading` to catch lazy loads in development.
- **Kitchen Sink Concern**: Concerns must be narrow and focused (e.g., `SoftDeletable`, `Sluggable`). If a concern exceeds ~30 lines or has multiple responsibilities, it's a service object in disguise.
