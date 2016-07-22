# ExtendedEmailReplyParser

[![Build Status](https://travis-ci.org/fiedl/extended_email_reply_parser.svg?branch=master)](https://travis-ci.org/fiedl/extended_email_reply_parser)

When implementing a "reply or comment by email" feature, it's neccessary to filter out signatures and the previous conversation. One needs to extract just the relevant parts for the conversation or comment section of the application. This is what this [ruby](https://www.ruby-lang.org) gem helps to do.

## Usage

### Parsing incoming emails

To extract the relevant text of an email reply, call `ExtendedEmailReplyParser.parse`, which accepts either a path to the email file, or a `Mail::Message` object, or the email body text itself, and returns the parsed, i.e. relevant text, which can be used as comment in the application

```ruby
ExtendedEmailReplyParser.parse "/path/to/email.eml"
ExtendedEmailReplyParser.parse message
ExtendedEmailReplyParser.parse body_text             # => parsed text as String
```

Or, if you prefer to call `#parse` on the `Mail::Message`:

```ruby
message.parse  # => parsed text as String
```

For example, for a incoming `Mail::Message`, `message`:

```ruby
@comment.author = User.where(email: message.from.first)
@comment.text = ExtendedEmailReplyParser.parse message
@comment.save!
```

### Extracting the body text

This gem extends [Mail::Message](https://github.com/mikel/mail/blob/master/lib/mail/message.rb) to  conveniently extract the text body as utf-8.

```ruby
message.extract_text
```

There's also a convenience method to extract the body text from an email file:

```ruby
ExtendedEmailReplyParser.read "/path/to/email.eml"  # => Mail::Message
ExtendedEmailReplyParser.read("/path/to/email.eml").extract_text  # => String
```

## Installation

Add this line to your application's Gemfile:

```ruby
# Gemfile
gem 'extended_email_reply_parser'
```

And then execute:

    ➜ bundle

Or install it yourself as:

    ➜ gem install extended_email_reply_parser

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/fiedl/extended_email_reply_parser.


## License

The gem is available as open source under the terms of the [MIT License](MIT-LICENSE).

