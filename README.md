# Slappy

[![Gem Version](https://badge.fury.io/rb/slappy.svg)](https://badge.fury.io/rb/slappy)
[![Build Status](https://travis-ci.org/yuemori/slappy.svg?branch=master)](https://travis-ci.org/yuemori/slappy)
[![Code Climate](https://codeclimate.com/repos/563cbaad1787d72930000582/badges/9753daa4ecd1a303b6ae/gpa.svg)](https://codeclimate.com/repos/563cbaad1787d72930000582/feed)
[![Test Coverage](https://codeclimate.com/repos/563cbaad1787d72930000582/badges/9753daa4ecd1a303b6ae/coverage.svg)](https://codeclimate.com/repos/563cbaad1787d72930000582/coverage)
[![Dependency Status](https://gemnasium.com/yuemori/slappy.svg)](https://gemnasium.com/yuemori/slappy)

This gem support to make slack bot with hubot like interface.
Use the Slack Realtime API(see the [official-documentation](https://api.slack.com/rtm)).

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'slappy'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install slappy

## Usage
### ENV
Store configuration value in environment variables. They are easy to change between deploys without changing any code.

```
SLACK_TOKEN - required (when not configured)
```

Slack API Token generated in [this page](https://api.slack.com/web).

### Configure
Configure default settings.
There configrations effect on send message to slack when use `say` method and should override when option given.

#### Example

```ruby
require 'slappy'

Slappy.configure do |config|
  config.username   = 'slappy'
  config.channel    = '#general'
  config.icon_emoji = ':slappy:'
end

Slappy.say 'hello!' #=> username: slappy, channel: '#general', icon_emoji: ':slappy:'
```

#### Configuration Parameters

```
token      - default: ENV['SLACK_TOKEN']
botname    - not effect now
username   - default: 'slappy'
icon_emoji - default: nil
channel    - default: '#general'
icon_url   - default: nil
```

### Example Code

```ruby
require 'slappy'

# called when start up
Slappy.hello do
  puts 'successfly connected'
end

# called when match message
Slappy.hear(/foo/) do
  puts 'foo'
end

# called when match message with pattern match
Slappy.hear(/bar (.*)/) do |event|
  puts event.matches[1] #=> Event#matches return MatchData object
end

# event object is slack event JSON (convert to [hashie](https://github.com/intridea/hashie))
Slappy.hear(/bar (.*)/) do |event|
  puts event.channel #=> channel id
  Slappy.say 'slappy!', channel: event.channel #=> to received message channel
  Slappy.say 'slappy!', channel: '#general'
  Slappy.say 'slappy!', username: 'slappy!', icon_emoji: ':slappy:'
end

Slappy.start
```

## How to run tests

Please create a file named .env on the root of this repository. You can use .env.example file as a template

```
cp .env.example .env
```

and edit `.env` file properly.

```
SLACK_TOKEN=abcd-1234567890-1234567890-1234567890
```

Then run tests.

```
bundle install
bundle exec rspec
```

## Feature

- [ ] Support private channel
- [ ] Support Schedule event (cron like)
- [ ] Generate template settings
- [ ] CLI commands
- [ ] Add bot name
- [ ] client#respond (hubot#respond)

## Contributing

1. Fork it ( http://github.com/aki017/slack-ruby-gem/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

