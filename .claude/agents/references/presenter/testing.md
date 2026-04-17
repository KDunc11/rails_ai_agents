# Decorator RSpec Tests

## Basic Decorator Tests

```ruby
# spec/decorators/entity_decorator_spec.rb
require 'rails_helper'

RSpec.describe EntityDecorator do
  let(:entity) { create(:entity, name: 'test entity', status: 'published', created_at: Time.zone.local(2024, 12, 25, 10, 30)) }
  let(:decorator) { described_class.decorate(entity) }

  describe '#formatted_name' do
    it 'titleizes the name' do
      expect(decorator.formatted_name).to eq('Test Entity')
    end
  end

  describe '#formatted_created_at' do
    it 'formats date with time' do
      expect(decorator.formatted_created_at).to eq('December 25, 2024 at 10:30 AM')
    end
  end

  describe '#status_class' do
    context 'when status is published' do
      it 'returns green text class' do
        expect(decorator.status_class).to eq('text-green-600')
      end
    end

    context 'when status is draft' do
      let(:entity) { create(:entity, status: 'draft') }

      it 'returns yellow text class' do
        expect(decorator.status_class).to eq('text-yellow-600')
      end
    end
  end

  describe '#display_description' do
    context 'when description is present' do
      let(:entity) { create(:entity, description: 'A great entity') }

      it 'returns the description' do
        expect(decorator.display_description).to eq('A great entity')
      end
    end

    context 'when description is blank' do
      let(:entity) { create(:entity, description: nil) }

      it 'returns fallback message' do
        expect(decorator.display_description).to eq('No description provided')
      end
    end
  end

  describe '#can_be_edited?' do
    context 'when status is draft' do
      let(:entity) { create(:entity, status: 'draft') }

      it 'returns true' do
        expect(decorator.can_be_edited?).to be true
      end
    end

    context 'when status is published' do
      it 'returns false' do
        expect(decorator.can_be_edited?).to be false
      end
    end
  end
end
```

## Testing with Additional Context

```ruby
# spec/decorators/user_decorator_spec.rb
require 'rails_helper'

RSpec.describe UserDecorator do
  let(:user) { create(:user, first_name: 'John', last_name: 'Doe', email: 'john@example.com') }
  let(:decorator) { described_class.decorate(user, context: { admin: true }) }

  describe '#display_name' do
    it 'returns full name when present' do
      expect(decorator.display_name).to eq('John Doe')
    end

    context 'when name is blank' do
      let(:user) { create(:user, first_name: nil, last_name: nil) }

      it 'returns email username' do
        expect(decorator.display_name).to eq('john')
      end
    end
  end

  describe '#avatar_tag' do
    it 'returns an image tag' do
      expect(decorator.avatar_tag).to include('<img')
      expect(decorator.avatar_tag).to include('avatar')
    end
  end
end
```

## Testing Number Formatting

```ruby
# spec/decorators/order_decorator_spec.rb
require 'rails_helper'

RSpec.describe OrderDecorator do
  let(:order) { create(:order, total: 12345.67) }
  let(:decorator) { described_class.decorate(order) }

  describe '#formatted_total' do
    it 'formats total as currency' do
      expect(decorator.formatted_total).to eq('$12,345.67')
    end
  end

  describe '#items_text' do
    context 'with one item' do
      let(:order) { create(:order, items_count: 1) }

      it 'returns singular form' do
        expect(decorator.items_text).to eq('1 item')
      end
    end

    context 'with multiple items' do
      let(:order) { create(:order, items_count: 5) }

      it 'returns plural form' do
        expect(decorator.items_text).to eq('5 items')
      end
    end
  end
end
```
