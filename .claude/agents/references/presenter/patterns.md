# Decorator Patterns

## ApplicationDecorator Base Class

```ruby
# app/decorators/application_decorator.rb
class ApplicationDecorator < Draper::Decorator
  delegate_all

  # define methods avaiable to all other decorators
  # private

  # def format_date(date)
  #   date.strftime("%B %d, %Y")
  # end
end
```

## Basic Decorator Structure

```ruby
# app/decorators/entity_decorator.rb
class EntityDecorator < ApplicationDecorator
  delegate_all

  # Delegate to the wrapped model
  delegate :id, :name, :status, :created_at, to: :object

  # Formatting methods
  def formatted_name
    name.titleize
  end

  def formatted_created_at
    created_at.strftime("%B %d, %Y at %I:%M %p")
  end

  def short_date
    created_at.strftime("%m/%d/%Y")
  end

  # Display logic
  def status_badge
    case status
    when 'published'
      h.content_tag(:span, 'Published', class: 'badge badge-success')
    when 'draft'
      h.content_tag(:span, 'Draft', class: 'badge badge-warning')
    when 'archived'
      h.content_tag(:span, 'Archived', class: 'badge badge-secondary')
    end
  end

  def status_class
    case status
    when 'published' then 'text-green-600'
    when 'draft' then 'text-yellow-600'
    when 'archived' then 'text-gray-600'
    else 'text-gray-400'
    end
  end

  # Conditional display
  def display_description
    description.presence || 'No description provided'
  end

  def can_be_edited?
    status == 'draft'
  end

  # Links
  def edit_link
    return nil unless can_be_edited?

    h.link_to 'Edit', h.edit_entity_path(object), class: 'btn btn-primary'
  end

  def delete_link
    return nil unless can_be_edited?

    h.link_to 'Delete',
              h.entity_path(object),
              method: :delete,
              data: { confirm: 'Are you sure?' },
              class: 'btn btn-danger'
  end
end
```

## User Decorator

```ruby
# app/decorators/user_decorator.rb
class UserDecorator < ApplicationDecorator
  delegate_all

  delegate :id, :email, :first_name, :last_name, :created_at, to: :object

  # Display name with fallback
  def display_name
    full_name.presence || email.split('@').first
  end

  def full_name
    "#{first_name} #{last_name}".strip
  end

  def initials
    parts = [first_name, last_name].compact
    return email[0].upcase if parts.empty?

    parts.map { |part| part[0].upcase }.join
  end

  # Avatar
  def avatar_url(size: 80)
    gravatar_hash = Digest::MD5.hexdigest(email.downcase)
    "https://www.gravatar.com/avatar/#{gravatar_hash}?s=#{size}&d=identicon"
  end

  def avatar_tag(size: 80, css_class: 'avatar')
    h.image_tag(avatar_url(size: size), alt: display_name, class: css_class)
  end

  # Formatting
  def member_since
    "Member since #{created_at.strftime('%B %Y')}"
  end

  def joined_recently?
    created_at > 30.days.ago
  end

  # Links
  def profile_link(css_class: nil)
    h.link_to display_name, h.user_path(object), class: css_class
  end
end
```

## Post Decorator with Rich Formatting

```ruby
# app/decorators/post_decorator.rb
class PostDecorator < ApplicationDecorator
  delegate_all

  delegate :id, :title, :body, :status, :published_at, :author, to: :object

  # Formatting
  def formatted_title
    title.titleize
  end

  def truncated_body(length: 200)
    return body if body.length <= length

    "#{body.truncate(length, separator: ' ')}..."
  end

  def reading_time
    words = body.split.size
    minutes = (words / 200.0).ceil
    "#{minutes} min read"
  end

  # Dates
  def published_date
    return 'Not published' unless published_at

    if published_at > 7.days.ago
      "#{time_ago_in_words(published_at)} ago"
    else
      published_at.strftime("%B %d, %Y")
    end
  end

  # Status
  def status_badge
    case status
    when 'published'
      h.content_tag(:span, 'Published', class: 'badge bg-success')
    when 'draft'
      h.content_tag(:span, 'Draft', class: 'badge bg-warning')
    else
      h.content_tag(:span, status.humanize, class: 'badge bg-secondary')
    end
  end

  # Author info
  def author_name
    author.present? ? author.full_name : 'Anonymous'
  end

  def byline
    "by #{author_name} on #{published_date}"
  end

  # Links
  def share_url
    h.url_for([object, host: h.request.host])
  end

  def twitter_share_link
    text = "Check out: #{formatted_title}"
    "https://twitter.com/intent/tweet?text=#{CGI.escape(text)}&url=#{CGI.escape(share_url)}"
  end
end
```

## Order Decorator with Number Formatting

```ruby
# app/decorators/order_decorator.rb
class OrderDecorator < ApplicationDecorator
  delegate_all

  delegate :id, :status, :total, :created_at, :items_count, to: :object

  # Currency formatting
  def formatted_total
    number_to_currency(total, precision: 2)
  end

  def formatted_subtotal
    number_to_currency(subtotal, precision: 2)
  end

  def formatted_tax
    number_to_currency(tax_amount, precision: 2)
  end

  # Number formatting
  def items_text
    "#{items_count} #{items_count == 1 ? 'item' : 'items'}"
  end

  # Status
  def status_text
    status.humanize
  end

  def status_color
    {
      'pending' => 'yellow',
      'paid' => 'green',
      'shipped' => 'blue',
      'delivered' => 'green',
      'cancelled' => 'red'
    }[status] || 'gray'
  end

  def status_badge
    h.content_tag(:span, status_text, class: "badge bg-#{status_color}-500")
  end

  # Dates
  def order_date
    created_at.strftime("%B %d, %Y")
  end

  def estimated_delivery
    return nil unless status.in?(%w[paid shipped])

    delivery_date = created_at + 5.business_days
    delivery_date.strftime("%B %d, %Y")
  end
end
```

## Booking Decorator with Conditional Logic

```ruby
# app/decorators/booking_decorator.rb
class BookingDecorator < ApplicationDecorator
  delegate_all

  delegate :id, :start_date, :end_date, :status, :user, to: :object

  # Status checks
  def active?
    status == 'confirmed' && end_date >= Date.today
  end

  def past?
    end_date < Date.today
  end

  def upcoming?
    active? && start_date > Date.today
  end

  def in_progress?
    active? && start_date <= Date.today && end_date >= Date.today
  end

  # Display logic
  def status_text
    return 'Cancelled' if status == 'cancelled'
    return 'Completed' if past?
    return 'In Progress' if in_progress?
    return 'Upcoming' if upcoming?

    status.humanize
  end

  def status_class
    return 'text-red-600' if status == 'cancelled'
    return 'text-gray-600' if past?
    return 'text-green-600' if in_progress?
    return 'text-blue-600' if upcoming?

    'text-gray-400'
  end

  # Date formatting
  def formatted_dates
    "#{start_date.strftime('%b %d')} - #{end_date.strftime('%b %d, %Y')}"
  end

  def duration_in_days
    (end_date - start_date).to_i + 1
  end

  def duration_text
    days = duration_in_days
    "#{days} #{days == 1 ? 'day' : 'days'}"
  end

  # Actions
  def can_cancel?
    active? && start_date > 1.day.from_now
  end

  def cancel_button
    return nil unless can_cancel?

    h.button_to 'Cancel Booking',
                h.cancel_booking_path(object),
                method: :post,
                data: { confirm: 'Are you sure?' },
                class: 'btn btn-danger'
  end
end
```

## Adding Context

If you need to pass extra data to your decorators, you can use a context hash. Methods that create decorators take it as an option, for example:

```ruby
Article.first.decorate(context: {role: :admin})
```

The value passed to the :context option is then available in the decorator through the context method, i.e.:

```ruby
class ArticleDecorator < ApplicationDecorator
  def delete_button
    return unless context[:role] == :admin

    # ...
  end
end
```

## Usage in Views

### Basic Usage

```erb
<%# app/views/entities/show.html.erb %>
<% decorator = @entity.decorate # or EntityDecorator.decorate(@entity) %>

<div class="entity-card">
  <h1><%= decorator.formatted_name %></h1>

  <div class="metadata">
    <%= decorator.status_badge %>
    <span class="date"><%= decorator.formatted_created_at %></span>
  </div>

  <p><%= decorator.display_description %></p>

  <div class="actions">
    <%= decorator.edit_link %>
    <%= decorator.delete_link %>
  </div>
</div>
```

### Collection Usage

```erb
<%# app/views/entities/index.html.erb %>
<% entities = @entities.decorate %>

<% entities.each do |entity| %>
  <div class="entity-item">
    <h3><%= entity.formatted_name %></h3>
    <%= entity.status_badge %>
  </div>
<% end %>
```
