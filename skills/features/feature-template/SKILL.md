---
name: feature-template
description: Skill derived from features/FEATURE_TEMPLATE.md
---

# Context Level 3

# Feature Specification Template

> Use this template to document a new feature BEFORE building it.
> This document guides the AI skill (or developers) during implementation.

## Skill workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                    📋 SPECIFICATION PHASE                       │
├─────────────────────────────────────────────────────────────────┤
│ 1. $feature-specification-agent → generates this document       │
│                         ↓                                       │
│ 2. $feature-reviewer-agent → review (score X/10)                │
│                         ↓                                       │
│    [If score < 7 or critical issues: revise]                    │
│                         ↓                                       │
│ 3. $feature-planner-agent → implementation plan                 │
├─────────────────────────────────────────────────────────────────┤
│                    🔴 RED PHASE (per PR)                        │
├─────────────────────────────────────────────────────────────────┤
│ 4. $tdd-red-agent → failing tests (Gherkin → RSpec)             │
├─────────────────────────────────────────────────────────────────┤
│                    🟢 GREEN PHASE (per PR)                      │
├─────────────────────────────────────────────────────────────────┤
│ 5. Specialist skills → minimal implementation                   │
│    • $model-agent, $migration-agent (database)                  │
│    • $service-agent, $form-agent (business logic)               │
│    • $policy-agent (authorization)                              │
│    • $controller-agent (endpoints)                              │
│    • $view-component-agent (UI components)                      │
│    • $sass-bem-agent (styling with SASS & BEM)                  │
│    • $mailer-agent, $job-agent (async)                          │
├─────────────────────────────────────────────────────────────────┤
│                    🔵 REFACTOR PHASE (per PR)                   │
├─────────────────────────────────────────────────────────────────┤
│ 6. $tdd-refactoring-agent → improves code (tests green)         │
│                         ↓                                       │
│ 7. $lint-agent → fixes style (Rubocop)                          │
├─────────────────────────────────────────────────────────────────┤
│                    ✅ REVIEW PHASE (per PR)                     │
├─────────────────────────────────────────────────────────────────┤
│ 8. $review-agent → code quality (SOLID, patterns)               │
│                         ↓                                       │
│ 9. $security-agent → security audit (Brakeman, vulnerabilities) │
│                         ↓                                       │
│    [If issues: go back to step 5 or 6]                          │
├─────────────────────────────────────────────────────────────────┤
│                    🚀 MERGE & DEPLOY                            │
├─────────────────────────────────────────────────────────────────┤
│ 10. Merge PR → integration branch                               │
│                         ↓                                       │
│     [Repeat 4-10 for each PR step]                              │
│                         ↓                                       │
│ 11. Merge feature branch → main                                 │
│                         ↓                                       │
│ 12. Deploy → production                                         │
└─────────────────────────────────────────────────────────────────┘
```

### Phase summary

| Phase | Skill(s) | Goal | Validation |
|-------|----------|------|------------|
| **Spec** | $feature-specification-agent | Create the spec | - |
| **Review Spec** | $feature-reviewer-agent | Validate the spec | Score ≥ 7/10 |
| **Plan** | $feature-planner-agent | Plan the implementation | - |
| **RED** | $tdd-red-agent | Write failing tests | Tests red |
| **GREEN** | Specialist skills | Minimal code | Tests green |
| **REFACTOR** | $tdd-refactoring-agent | Improve code | Tests green |
| **LINT** | $lint-agent | Style & formatting | Rubocop clean |
| **REVIEW** | $review-agent | Code quality | No HIGH/CRITICAL issues |
| **SECURITY** | $security-agent | Security audit | Brakeman clean |
| **MERGE** | Developer | Integrate code | CI green |

---

## 📋 General information

**Feature name:** `[Short, descriptive name]`

**Ticket/Issue:** `#[number]`

**Priority:** `[High / Medium / Low]`

**Estimate:** `[Small / Medium / Large]` or `[X days]`

---

## 🎯 Objective

**Problem to solve:**
> Describe in 2-3 sentences the business or user problem this feature solves.
> Example: "Users cannot filter restaurants by cuisine type, making search difficult when they have a specific craving."

**Value delivered:**
> What concrete benefit for the user or the business?
> Example: "Improves user experience and increases conversion by 15%."

**Success criteria:**
- [ ] Measurable criterion 1
- [ ] Measurable criterion 2
- [ ] Measurable criterion 3

---

## 👤 Affected personas

Check the personas impacted:
- [ ] Visitor (not authenticated)
- [ ] Signed-in user
- [ ] Resource owner (Entity Owner)
- [ ] Admin

> 📋 **For each checked persona**, document permissions in the Policies section below.

### Authorization matrix

| Action | Visitor | User | Owner | Admin |
|--------|---------|------|-------|-------|
| View | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Create | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Update | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |
| Delete | ✅/❌ | ✅/❌ | ✅/❌ | ✅/❌ |

---

## 📝 User stories

### Primary story
```
As a [persona],
I want [action],
So that [benefit].
```

**Acceptance criteria:**
- [ ] Criterion 1 (measurable, yes/no verifiable)
- [ ] Criterion 2 (measurable, yes/no verifiable)
- [ ] Criterion 3 (measurable, yes/no verifiable)

> ⚠️ **Note:** Criteria must be testable and avoid subjective terms like "good", "fast", "intuitive".

### Gherkin scenarios (Acceptance Criteria)

> 📋 These scenarios are the basis for acceptance tests with `$tdd-red-agent`.

```gherkin
Feature: [Feature name]

  Background:
    Given [shared context]

  # Happy Path
  Scenario: [Primary success scenario]
    Given [precondition]
    When [user action]
    Then [expected result]
    And [additional verification]

  # Validation
  Scenario: [Data validation]
    Given [precondition]
    When [action with invalid data]
    Then [error message shown]
    And [data preserved in the form]

  # Authorization
  Scenario: [Access control]
    Given [unauthorized user]
    When [attempt protected action]
    Then [redirect or error message]
```

### Secondary stories (optional)
> If the feature is complex, list other stories with their own Gherkin scenarios.

---

## ⚠️ Edge cases & error handling

> 🔴 **REQUIRED:** Document at least 3 edge cases.

### Identified edge cases

| # | Type | Scenario | Expected behavior | Error message |
|---|------|----------|-------------------|---------------|
| 1 | Invalid input | [Description] | [Behavior] | [Message] |
| 2 | Unauthorized access | [Description] | [Behavior] | [Message] |
| 3 | Empty/null state | [Description] | [Behavior] | [Message] |
| 4 | Network/system error | [Description] | [Behavior] | [Message] |
| 5 | Concurrent operation | [Description] | [Behavior] | [Message] |

### Gherkin scenarios for edge cases

```gherkin
  # Edge Case: Invalid Input
  Scenario: User submits invalid data
    Given [precondition]
    When [action with invalid data]
    Then [expected behavior]
    And [specific error message]

  # Edge Case: Unauthorized Access
  Scenario: Unauthorized user attempts action
    Given I am logged in as [unauthorized persona]
    When I attempt to [protected action]
    Then I should see "[error message]"
    And I should be redirected to [destination]

  # Edge Case: Empty State
  Scenario: No data available
    Given [no data exists]
    When I visit [page]
    Then I should see "[empty state message]"
    And I should see [call to action]
```

---

## 🔄 Incremental PR breakdown

> ⚠️ **IMPORTANT:** Never ship a large feature in a single PR.
>
> This section is **required** for any feature estimated at more than one dev day.

### Integration branch

**Branch name:** `feature/[feature-name]`

This branch contains the full feature and is merged into `main` only after all incremental PRs are validated.

### Breakdown plan

> Split the feature into **5-10 small PRs** max (ideally 3-5).
> Each PR must:
> - Be under 400 lines (ideally 50-200)
> - Have a single clear objective
> - Be functional and tested (even if the feature is incomplete)
> - Target the integration branch (not main)

#### Step 1: [Short title]
**Branch:** `feature/[name]-step-1-[description]`

**Goal:**
> One sentence description of what this PR does.
> Example: "Add the migration and cuisine_type column to restaurants"

**Contents:**
- [ ] Migration `add_cuisine_type_to_restaurants`
- [ ] Index on the column
- [ ] Migration tests (up/down)

**Estimate:** 30 min dev + 15 min review

**Tests included:**
- [ ] Reversible migration
- [ ] Index created correctly

---

#### Step 2: [Short title]
**Branch:** `feature/[name]-step-2-[description]`

**Goal:**
> Example: "Add validations and filtering scope to Restaurant model"

**Contents:**
- [ ] Constant `CUISINE_TYPES`
- [ ] Validation `inclusion` on `cuisine_type`
- [ ] Scope `by_cuisine`
- [ ] Model unit tests

**Estimate:** 1h dev + 30 min review

**Tests included:**
- [ ] Validation tests
- [ ] Scope tests
- [ ] Edge cases (nil, invalid value)

---

#### Step 3: [Short title]
**Branch:** `feature/[name]-step-3-[description]`

**Goal:**
> Example: "Update controller to accept cuisine filter"

**Contents:**
- [ ] Modify `RestaurantsController#index`
- [ ] Add `cuisine` param to strong params
- [ ] Request spec tests

**Estimate:** 1h dev + 30 min review

**Tests included:**
- [ ] Controller tests with/without filter
- [ ] Authorization tests if applicable

---

#### Step 4: [Short title]
**Branch:** `feature/[name]-step-4-[description]`

**Goal:**
> Example: "Add UI filtering interface"

**Contents:**
- [ ] Filter form in `index.html.haml`
- [ ] Turbo Frame for dynamic reload
- [ ] Tailwind styling

**Estimate:** 2h dev + 1h review

**Tests included:**
- [ ] Feature tests with Capybara
- [ ] JavaScript tests if interactions are complex

---

#### Step 5: [Short title] (optional)
**Branch:** `feature/[name]-step-5-[description]`

**Goal:**
> Example: "End-to-end integration tests and documentation"

**Contents:**
- [ ] Full integration tests
- [ ] Updated documentation
- [ ] Updated seeds

**Estimate:** 1h dev + 30 min review

**Tests included:**
- [ ] Full user scenario
- [ ] Regression tests

---

### Merge strategy

```bash
# 1. Create the integration branch
git checkout -b feature/[feature-name]
git push -u origin feature/[feature-name]

# 2. For each step:
git checkout feature/[feature-name]
git checkout -b feature/[name]-step-X-[description]
# ... develop ...
git commit -m "feat: step X description"
git push -u origin feature/[name]-step-X-[description]

# 3. Create a PR to the integration branch
gh pr create --base feature/[feature-name] \
  --title "[Step X/Y] Short description" \
  --body "Part of #[issue]. Detailed description."

# 4. Review + merge the step
# 5. Repeat for each step

# 6. Once all steps are merged:
gh pr create --base main \
  --title "Feature: [Full feature name]" \
  --body "Closes #[issue]. All incremental PRs reviewed and merged."
```

### Breakdown checklist

- [ ] Feature is split into **3-10 steps maximum**
- [ ] Each step is **under 400 lines**
- [ ] Each step is **self-contained and tested**
- [ ] Step order is **logical** (dependencies respected)
- [ ] Each step has a **time estimate**
- [ ] The **full plan** is documented before starting

### Breakdown rules

#### ✅ Good breakdown
- Migration only (step 1)
- Model + validations (step 2)
- Controller + routes (step 3)
- Views + components (step 4)
- Integration tests (step 5)

#### ❌ Bad breakdown
- Migration + model + controller + views (too big)
- Validations only without tests (incomplete)
- Half a controller (not self-contained)
- All tests at the end (risky)

### For coding skills

When using a coding skills (Claude Code, GitHub Copilot, OpenAI Codex, etc.):

**❌ Do not ask:**
```
"Implement the entire feature [name]"
```

**✅ Ask instead:**
```
"Implement Step 1 of the feature spec [name]: [step 1 description]"
```

Then once Step 1 is reviewed and merged:
```
"Implement Step 2 of the feature spec [name]: [step 2 description]"
```

And so on.

**Benefits:**
- 🎯 Focused context → fewer errors
- ✅ Fast review → immediate feedback
- 🔁 Easy corrections → no full rework
- 📈 Visible progress → team confidence

---

## 🏗️ Technical scope

### Impacted models

#### New models
```ruby
# If creating a new model
class NewModel < ApplicationRecord
  # Primary attributes
  # - attribute_name: type (constraints)

  # Associations
  # belongs_to :xxx
  # has_many :yyy

  # Core validations
  # validates :xxx, presence: true
end
```

#### Changes to existing models
**Model:** `ExistingModel`

**Changes:**
- [ ] Add attribute: `new_attribute:string`
- [ ] Add relation: `has_many :new_relation`
- [ ] New validation: `validates :xxx, ...`
- [ ] New scope: `scope :by_xxx, -> { ... }`
- [ ] New method: `def calculate_xxx`

### Validation rules

> 🔴 **REQUIRED:** For each user-facing field, specify validation rules.

| Field | Type | Required | Validation rules | Error message |
|-------|------|----------|------------------|---------------|
| `name` | string | Yes | presence, length: 2..100 | "Name is required" |
| `email` | string | Yes | format: URI::MailTo::EMAIL_REGEXP | "Invalid email format" |
| `amount` | decimal | Yes | numericality: { greater_than: 0 } | "Amount must be positive" |
| `status` | string | Yes | inclusion: { in: STATUSES } | "Invalid status" |
| `description` | text | No | length: { maximum: 1000 } | "Description too long (max 1000)" |

### Migration(s)

```ruby
# db/migrate/YYYYMMDDHHMMSS_add_feature_name.rb
class AddFeatureName < ActiveRecord::Migration[8.1]
  def change
    # Add columns
    add_column :table_name, :column_name, :type, null: false, default: value

    # Add index
    add_index :table_name, :column_name

    # Create table
    create_table :new_table do |t|
      t.string :name, null: false
      t.references :parent, foreign_key: true
      t.timestamps
    end
  end
end
```

**⚠️ Migration checklist:**
- [ ] Reversible migration (`up`/`down` or `change`)
- [ ] Indexes added on key columns
- [ ] Defaults set if required
- [ ] Foreign keys with proper `on_delete`

### Controllers

#### New controllers
- `NewController` with actions: `index`, `show`, `new`, `create`, `edit`, `update`, `destroy`

#### Changes to existing controllers
**Controller:** `ExistingController`

**Changes:**
- [ ] New action: `custom_action`
- [ ] Update strong parameters
- [ ] Add before_action
- [ ] Update business logic

**Strong parameters:**
```ruby
def model_params
  params.require(:model_name).permit(:attr1, :attr2, :attr3)
end
```

### Routes

```ruby
# config/routes.rb
resources :resource_name do
  # Nested routes if needed
  resources :nested_resource, only: [:index, :create, :destroy]

  # Custom routes
  member do
    post :custom_action
  end

  collection do
    get :custom_collection_action
  end
end
```

### Services (if logic is complex)

**Service:** `FeatureNameService`

**Responsibility:**
> Describe in 1-2 sentences what this service does.

**Key methods:**
```ruby
class FeatureNameService
  def initialize(params)
    @params = params
  end

  def call
    # Complex business logic here
    # Returns a result or raises an exception
  end

  private

  def step_one
    # ...
  end
end
```

### Policies (Pundit)

**Policy:** `ModelPolicy`

**New rules:**
```ruby
class ModelPolicy < ApplicationPolicy
  def action_name?
    # user.admin? || record.user == user
  end
end
```

### Views & components

#### New views
- `app/views/resource_name/index.html.haml`
- `app/views/resource_name/show.html.haml`
- `app/views/resource_name/_form.html.haml`

#### New components
**Component:** `FeatureNameComponent`

```ruby
class FeatureNameComponent < ViewComponent::Base
  def initialize(param:)
    @param = param
  end

  def render?
    # Render condition
  end
end
```

#### Changes to existing views
- [ ] View to modify: `path/to/view.html.haml`
- [ ] Type of change: [Add link / New form / Data display]

### JavaScript (Stimulus)

#### New Stimulus controllers
**Controller:** `feature_name_controller.js`

```javascript
import { Controller } from "@hotwire/stimulus"

export default class extends Controller {
  static targets = ["element"]
  static values = { param: String }

  connect() {
    // Initialization
  }

  action() {
    // Logic
  }
}
```

### Jobs (Background)

**Job:** `FeatureNameJob`

```ruby
class FeatureNameJob < ApplicationJob
  queue_as :default

  def perform(param)
    # Async processing
  end
end
```

**Trigger:**
- Where: `ModelName#method_name`
- When: `after_commit :enqueue_job`

---

## 🧪 Test strategy

### Model tests (RSpec)

**File:** `spec/models/model_name_spec.rb`

**Tests to write:**
- [ ] Validations (presence, format, uniqueness, etc.)
- [ ] Associations (belongs_to, has_many, etc.)
- [ ] Scopes (verify SQL queries)
- [ ] Business methods (logic, edge cases)
- [ ] Callbacks (after_save, before_destroy, etc.)

**Example tests:**
```ruby
RSpec.describe ModelName, type: :model do
  describe "validations" do
    it { should validate_presence_of(:attribute) }
    it { should validate_uniqueness_of(:attribute) }
  end

  describe "#custom_method" do
    it "returns expected result" do
      instance = create(:model_name)
      expect(instance.custom_method).to eq(expected_value)
    end
  end
end
```

### Controller tests (Request specs)

**File:** `spec/requests/controller_name_spec.rb`

**Tests to write:**
- [ ] CRUD actions (index, show, create, update, destroy)
- [ ] Authorizations (signed-in user, owner, etc.)
- [ ] Redirects and flash messages
- [ ] HTTP responses (200, 302, 404, 422, etc.)

**Example tests:**
```ruby
RSpec.describe "ResourceName", type: :request do
  let(:user) { create(:user) }
  let(:resource) { create(:resource_name, user: user) }

  describe "GET /resources/:id" do
    it "returns http success" do
      get resource_path(resource)
      expect(response).to have_http_status(:success)
    end
  end

  describe "POST /resources" do
    context "with valid params" do
      it "creates a new resource" do
        expect {
          post resources_path, params: { resource_name: valid_attributes }
        }.to change(ResourceName, :count).by(1)
      end
    end
  end
end
```

### Integration tests (Feature specs)

**File:** `spec/features/feature_name_spec.rb`

**Scenarios to test:**
- [ ] Full user flow (happy path)
- [ ] Error cases (invalid form, access denied)
- [ ] JavaScript interactions (if applicable)

**Example tests:**
```ruby
RSpec.describe "Feature Name", type: :feature do
  scenario "user completes the feature workflow" do
    user = create(:user)
    login_as(user)

    visit new_resource_path
    fill_in "Name", with: "Example"
    click_button "Create"

    expect(page).to have_content("Resource created successfully")
    expect(page).to have_current_path(resource_path(ResourceName.last))
  end
end
```

### Component tests

**File:** `spec/components/component_name_component_spec.rb`

**Tests to write:**
- [ ] Rendering with different params
- [ ] Display conditions (`render?`)
- [ ] Generated content

### Policy tests

**File:** `spec/policies/policy_name_spec.rb`

**Tests to write:**
- [ ] Permissions by role
- [ ] Edge cases

```ruby
RSpec.describe ResourcePolicy, type: :policy do
  subject { described_class.new(user, resource) }

  context "for owner" do
    let(:user) { resource.user }
    it { should permit_action(:update) }
    it { should permit_action(:destroy) }
  end

  context "for other user" do
    let(:user) { create(:user) }
    it { should_not permit_action(:update) }
  end
end
```

---

## 🔒 Security considerations

- [ ] **Strong parameters**: all attributes are filtered
- [ ] **Pundit authorization**: all actions are protected
- [ ] **Validations**: all user inputs are validated
- [ ] **SQL injection**: use ActiveRecord, no raw SQL
- [ ] **XSS**: use Rails helpers (sanitize, escape)
- [ ] **CSRF**: tokens present on forms
- [ ] **Mass assignment**: use `permit` correctly
- [ ] **Sensitive data**: no logging or display of secrets

---

## ⚡ Performance considerations

- [ ] **N+1 queries**: use `includes`/`preload`/`eager_load`
- [ ] **DB indexes**: add indexes on queried columns
- [ ] **Cache**: identify data to cache
- [ ] **Background jobs**: move long tasks async
- [ ] **Pagination**: limit list results
- [ ] **Heavy queries**: optimize with `select`, `pluck`, `exists?`

---

## 📱 UI/UX considerations

> 🔴 **REQUIRED for UI features:** Document interactive states.

### UI/UX checklist
- [ ] **Responsive**: design fits mobile/tablet/desktop
- [ ] **Accessibility**: labels, aria-labels, contrast (WCAG 2.1 AA minimum)
- [ ] **User feedback**: flash messages, loading states
- [ ] **Client validation**: Stimulus + HTML5 validation
- [ ] **Error handling**: clear, actionable error messages

### Interactive states (Hotwire/Turbo)

| State | Description | Implementation |
|------|-------------|----------------|
| **Loading** | While loading | Turbo Frame with spinner, `aria-busy="true"` |
| **Success** | Action succeeded | Flash notice, Turbo Stream append/replace |
| **Error** | Action failed | Flash alert, form preserved, inline errors |
| **Empty** | No data | Explanatory message + call to action |
| **Disabled** | Action unavailable | Disabled button + explanatory tooltip |

### User messages

| Context | Type | Message |
|---------|------|---------|
| Creation success | success | "[Resource] created successfully" |
| Update success | success | "[Resource] updated" |
| Deletion success | success | "[Resource] deleted" |
| Validation error | error | "Please fix the errors below" |
| Unauthorized | error | "You are not authorized to perform this action" |
| Not found | error | "[Resource] not found" |

---

## 🚀 Deployment plan

### Prerequisites
- [ ] Migration tested (up & down)
- [ ] Seeds updated if needed
- [ ] Assets precompiled (if CSS/JS changes)
- [ ] Environment variables added (if needed)

### Steps
1. Deploy code
2. Run migrations: `rails db:migrate`
3. Restart workers if jobs were added
4. Check logs
5. Test in production

### Rollback plan
> How to roll back if something breaks?
```bash
# Roll back migration
rails db:rollback STEP=1

# Redeploy previous version
kamal rollback
```

---

## 📚 Documentation to update

- [ ] `README.md`: if major feature
- [ ] `.github/project.md`: if new core functionality
- [ ] `.github/CONTRIBUTING.md`: if new conventions
- [ ] API docs: if endpoints exposed
- [ ] User guide: if user-visible feature

---

## ✅ Final checklist before merge

### Code
- [ ] Code written and working
- [ ] Rubocop passes without errors
- [ ] No commented-out code or `binding.pry`
- [ ] Naming conventions followed

### Tests
- [ ] All tests pass
- [ ] Coverage maintained (>90%)
- [ ] Unit tests written
- [ ] Integration tests written
- [ ] Edge cases tested

### Security
- [ ] Brakeman reports no new vulnerabilities
- [ ] Bundler Audit OK
- [ ] Policies tested
- [ ] Strong parameters verified

### Documentation
- [ ] Code commented if logic is complex
- [ ] README updated if needed
- [ ] CHANGELOG.md updated

### Review
- [ ] PR created with clear description
- [ ] Screenshots/GIFs if UI changes
- [ ] Reviewer assigned
- [ ] CI/CD green

---

## 💡 Notes & questions

> Free space to capture questions, technical decisions, or special concerns.

**Open questions:**
-

**Technical decisions:**
-

**Points of attention:**
-

**External dependencies:**
-

---

**Creation date:** `[YYYY-MM-DD]`

**Author:** `[@username]`

**Reviewers:** `[@username1, @username2]`

**Status:** `[Draft / In Review / Ready for Dev / In Progress / Completed]`

---

## 📋 Review criteria ($feature-reviewer-agent)

> This section summarizes what `$feature-reviewer-agent` will check.

### MUST HAVE (blocking if missing)
- [ ] Objective and value clearly stated
- [ ] Personas identified
- [ ] Primary user story documented
- [ ] Testable acceptance criteria (yes/no verifiable)
- [ ] Gherkin scenarios for acceptance tests
- [ ] Edge cases documented (minimum 3)
- [ ] Authorization matrix completed

### SHOULD HAVE (recommended)
- [ ] Validation rules table
- [ ] Technical components listed
- [ ] Database changes documented
- [ ] Pundit policies specified
- [ ] Integration points identified

### IF UI (required for UI features)
- [ ] Loading/error/empty/success states documented
- [ ] User messages defined
- [ ] Responsive behavior specified
- [ ] Accessibility considered (WCAG 2.1 AA)

### IF Medium/Large (required if > 1 day)
- [ ] PR breakdown (3-10 steps)
- [ ] Each PR < 400 lines (ideally 50-200)
- [ ] Dependencies between PRs are clear
- [ ] Tests included in each PR
```
