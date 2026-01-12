# AGENTS.md

This repo uses local agent docs and templates to guide AI assistance.

## Locations

All locations are nested under the `.codex/` directory in this repository.

- `agents/`
- `37signals_agents/`
- `commands/`
- `feature_spec_agents/`
- `features/`

## How to use in prompts

When asking for help, include one of these:
- “Use `agents/<agent>.md` for this task.”
- “Use `37signals_agents/<agent>.md` for this task.”
- “Use `feature_spec_agents/<agent>.md` and `features/FEATURE_TEMPLATE.md`.”
- “Follow `AGENTS.md`.”

If multiple docs apply, list them in order of priority.

## Available resources

### agents/
- `agents/controller-agent.md`
- `agents/form-agent.md`
- `agents/job-agent.md`
- `agents/mailer-agent.md`
- `agents/model-agent.md`
- `agents/policy-agent.md`
- `agents/decorator-agent.md`
- `agents/query-agent.md`
- `agents/review-agent.md`
- `agents/rspec-agent.md`
- `agents/scss-bem-agent.md`
- `agents/security-agent.md`
- `agents/service-agent.md`
- `agents/stimulus-agent.md`
- `agents/turbo-agent.md`
- `agents/view-component-agent.md`

### 37signals_agents/
- `37signals_agents/auth-agent.md`
- `37signals_agents/caching-agent.md`
- `37signals_agents/concerns-agent.md`
- `37signals_agents/crud-agent.md`
- `37signals_agents/events-agent.md`
- `37signals_agents/implement-agent.md`
- `37signals_agents/jobs-agent.md`
- `37signals_agents/mailer-agent.md`
- `37signals_agents/migration-agent.md`
- `37signals_agents/model-agent.md`
- `37signals_agents/multi-tenant-agent.md`
- `37signals_agents/refactoring-agent.md`
- `37signals_agents/review-agent.md`
- `37signals_agents/state-records-agent.md`
- `37signals_agents/stimulus-agent.md`
- `37signals_agents/test-agent.md`
- `37signals_agents/turbo-agent.md`

### commands/
- `commands/frame-problem.md`
- `commands/refine-specification.md`

### feature_spec_agents/
- `feature_spec_agents/feature-planner-agent.md`
- `feature_spec_agents/feature-reviewer-agent.md`
- `feature_spec_agents/feature-specification-agent.md`

### features/
- `features/FEATURE_TEMPLATE.md`

## Project conventions (quick reminders)

- Views/templates: HAML
- Forms: `simple_form_for`
- Decorators: Draper, all inherit from `ApplicationDecorator`
- Background jobs: Sidekiq
- CSS: SCSS + BEM


## Agent Tags

These agents are referenced by tag in prompts (Copilot-style). Use these tags in your request and the corresponding file
will be followed.

### How to use
- “Use `@model_agent` and `@form_agent`.”
- “Use `@37signals/turbo_agent`.”
- “Use `@feature_planner_agent` and follow `features/FEATURE_TEMPLATE.md`.”

**Note:** Agent tags are required to trigger agent-specific behavior. Always include the tag (e.g., `@model_agent`) in
your prompt.

### Tag → File mapping

#### Standard agents (`agents/`)
- `@controller_agent` → `agents/controller-agent.md`
- `@form_agent` → `agents/form-agent.md`
- `@job_agent` → `agents/job-agent.md`
- `@mailer_agent` → `agents/mailer-agent.md`
- `@model_agent` → `agents/model-agent.md`
- `@policy_agent` → `agents/policy-agent.md`
- `@decorator_agent` → `agents/decorator-agent.md` (Draper decorators)
- `@query_agent` → `agents/query-agent.md`
- `@review_agent` → `agents/review-agent.md`
- `@rspec_agent` → `agents/rspec-agent.md`
- `@scss_bem_agent` → `agents/scss-bem-agent.md`
- `@security_agent` → `agents/security-agent.md`
- `@service_agent` → `agents/service-agent.md`
- `@stimulus_agent` → `agents/stimulus-agent.md`
- `@turbo_agent` → `agents/turbo-agent.md`
- `@view_component_agent` → `agents/view-component-agent.md`

#### 37signals agents (`37signals_agents/`)
- `@37signals/auth_agent` → `37signals_agents/auth-agent.md`
- `@37signals/caching_agent` → `37signals_agents/caching-agent.md`
- `@37signals/concerns_agent` → `37signals_agents/concerns-agent.md`
- `@37signals/crud_agent` → `37signals_agents/crud-agent.md`
- `@37signals/events_agent` → `37signals_agents/events-agent.md`
- `@37signals/implement_agent` → `37signals_agents/implement-agent.md`
- `@37signals/jobs_agent` → `37signals_agents/jobs-agent.md`
- `@37signals/mailer_agent` → `37signals_agents/mailer-agent.md`
- `@37signals/migration_agent` → `37signals_agents/migration-agent.md`
- `@37signals/model_agent` → `37signals_agents/model-agent.md`
- `@37signals/multi_tenant_agent` → `37signals_agents/multi-tenant-agent.md`
- `@37signals/refactoring_agent` → `37signals_agents/refactoring-agent.md`
- `@37signals/review_agent` → `37signals_agents/review-agent.md`
- `@37signals/state_records_agent` → `37signals_agents/state-records-agent.md`
- `@37signals/stimulus_agent` → `37signals_agents/stimulus-agent.md`
- `@37signals/test_agent` → `37signals_agents/test-agent.md`
- `@37signals/turbo_agent` → `37signals_agents/turbo-agent.md`

#### Feature spec agents (`feature_spec_agents/`)
- `@feature_specification_agent` → `feature_spec_agents/feature-specification-agent.md`
- `@feature_planner_agent` → `feature_spec_agents/feature-planner-agent.md`
- `@feature_reviewer_agent` → `feature_spec_agents/feature-reviewer-agent.md`

#### Commands / templates
- `commands/frame-problem.md`
- `commands/refine-specification.md`
- `features/FEATURE_TEMPLATE.md`
