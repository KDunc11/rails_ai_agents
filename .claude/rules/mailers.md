---
paths:
  - "app/mailers/**/*.rb"
  - "app/views/**/*_mailer/**/*.erb"
  - "spec/mailers/**/*.rb"
---

# Mailer Conventions

- Always provide both HTML and text templates
- Use `perform_async` (async via Sidekiq), never `perform` in controllers
- Create mailer previews in `spec/mailers/previews/` or `test/mailers/previews/`
- Test with `have_enqueued_mail(MailerClass, :method_name)`
- Keep mailer logic minimal -- formatting belongs in decorators
