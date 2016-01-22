## Stop Versioning Rails Secret Tokens

After reading Code Climate's [Rails' Insecure Defaults](http://blog.codeclimate.com/blog/2013/03/27/rails-insecure-defaults/) I realized I was [guilty](https://github.com/cjolly/chadjolly.com/blob/8f8b7ee89b788bea000c85871eac52c27d33c878/config/initializers/secret_token.rb) of breaking rule 3. Versioned Secret Tokens. Here's how I [fixed](https://github.com/cjolly/chadjolly.com/commit/f532386e36f95fbf47388a6fc08414657a0f0b39) it.

Use [dotenv](https://github.com/bkeepers/dotenv) in development and test environments:
```ruby
# Gemfile
gem 'dotenv-rails', groups: [:development, :test]
```

Local development key for dotenv:
```shell
echo RAILS_SECRET_KEY_BASE=`rake secret` > .env
```

Secure rails initializer:
```ruby
# config/initializers/secret_token.rb
YourApp::Application.config.secret_key_base = ENV['RAILS_SECRET_KEY_BASE']
```

Securely set key on heroku. Keep your key out of your shell history and buffer:
```bash
heroku config:set RAILS_SECRET_KEY_BASE=`rake secret` > /dev/null
```