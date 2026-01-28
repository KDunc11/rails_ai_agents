---
name: sidekiq-setup
description: Configures Sidekiq for background jobs in Rails. Use when setting up Sidekiq, creating jobs, configuring queues, or adding recurring jobs.
allowed-tools: Read, Write, Edit, Bash
---

# Sidekiq Setup for Rails

## Overview

Sidekiq is a Redis-backed background job processor:
- Multi-threaded, high throughput
- Queue priorities and weights
- Built-in retries and error handling
- Optional recurring jobs via sidekiq-cron or sidekiq-scheduler
- Web UI for monitoring

## Quick Start

### Installation

```bash
# Add to Gemfile
bundle add sidekiq

# Start Redis (if not already running)
redis-server
```

### Configuration

```yaml
# config/sidekiq.yml
:concurrency: 10
:queues:
  - critical
  - default
  - low
  - mailers
```

### Set as Active Job Adapter (optional)

```ruby
# config/application.rb
config.active_job.queue_adapter = :sidekiq

# Or per environment
# config/environments/production.rb
config.active_job.queue_adapter = :sidekiq
```

## Workflow Checklist

```
Sidekiq Setup:
- [ ] Add sidekiq gem
- [ ] Start Redis
- [ ] Configure queues in sidekiq.yml
- [ ] Wire Sidekiq Web UI (optional)
- [ ] Create first job with spec
- [ ] Test job execution
- [ ] Configure recurring jobs (if needed)
```

## Creating Jobs

### Basic Job

```ruby
# app/workers/send_welcome_email_worker.rb
class SendWelcomeEmailWorker
  include Sidekiq::Worker

  sidekiq_options queue: :default, retry: 3

  def perform(user_id)
    user = User.find(user_id)
    UserMailer.welcome(user).deliver_now
  end
end
```

### Job with Custom Retry and Timeout

```ruby
# app/workers/process_payment_worker.rb
class ProcessPaymentWorker
  include Sidekiq::Worker

  sidekiq_options queue: :critical, retry: 5

  def perform(order_id)
    order = Order.find(order_id)

    Timeout.timeout(30) do
      PaymentService.new.charge(order)
    end
  rescue PaymentGatewayError
    raise # retry
  rescue ActiveRecord::RecordNotFound
    # discard
  end
end
```

### Job with Idempotency Guard

```ruby
class CalculateMetricsWorker
  include Sidekiq::Worker

  sidekiq_options queue: :default, retry: 5

  def perform(entity_id)
    entity = Entity.find_by(id: entity_id)
    return unless entity

    average = entity.submissions.average(:rating).to_f.round(1)
    count = entity.submissions.count
    entity.update!(average_score: average, submissions_count: count)
  end
end
```

## Enqueueing Jobs

```ruby
# Enqueue immediately
CalculateMetricsWorker.perform_async(entity.id)

# Enqueue with delay
SendReminderWorker.perform_in(1.hour, user.id)

# Enqueue at specific time
SendReportWorker.perform_at(Date.tomorrow.noon, user.id)
```

## Recurring Jobs

```yaml
# config/sidekiq_schedule.yml
send_daily_digest:
  class: SendDailyDigestWorker
  cron: "0 8 * * *"
  queue: mailers

cleanup_old_data:
  class: CleanupOldDataWorker
  cron: "0 2 * * *"
  queue: maintenance
```

## Testing Jobs

### Sidekiq Test Setup

```ruby
# spec/spec_helper.rb
require "sidekiq/testing"

Sidekiq::Testing.fake!

RSpec.configure do |config|
  config.before do
    Sidekiq::Job.clear_all
  end
end
```

### Job Spec Template

```ruby
# spec/workers/calculate_metrics_worker_spec.rb
require "rails_helper"

RSpec.describe CalculateMetricsWorker, type: :worker do
  let(:entity) { create(:entity) }

  describe "#perform" do
    it "calculates metrics idempotently" do
      described_class.new.perform(entity.id)
      described_class.new.perform(entity.id)

      entity.reload
      expect(entity.average_score).to be_present
    end
  end

  describe "enqueueing" do
    it "enqueues the worker" do
      Sidekiq::Testing.fake! do
        worker = described_class.perform_async(entity.id)

        expect(worker["args"]).to eq([entity.id])
        expect(worker["queue"]).to eq("default")
      end
    end
  end
end
```

## Running Sidekiq

```bash
# Development
bundle exec sidekiq -C config/sidekiq.yml

# Production (via Procfile)
# Procfile
web: bin/rails server
worker: bundle exec sidekiq -C config/sidekiq.yml
```

## Monitoring

### Sidekiq Web UI

```ruby
# config/routes.rb
require "sidekiq/web"
mount Sidekiq::Web => "/sidekiq"
```

### Console Queries

```ruby
# Check queue sizes
Sidekiq::Queue.all.map { |q| [q.name, q.size] }

# Check retries
Sidekiq::RetrySet.new.size

# Check dead jobs
Sidekiq::DeadSet.new.size
```

## Best Practices

- Make jobs idempotent and pass IDs, not AR objects
- Keep jobs small and resilient
- Configure explicit queues and retries
- Log key steps and error context
- Avoid assuming execution order
