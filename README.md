# ActionTexter

Generic interface to send SMSs with Ruby.

## Installation

Add this line to your application's Gemfile:

    gem 'action_texter'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install action_texter

## Usage

TODO: Write better usage instructions here

1. Configure which client to use in your initializers

```
if Rails.env.test? # || Rails.env.development?
  ActionTexter::Client.setup("Test")
else
  ActionTexter::Client.setup("Nexmo", "key", "secret")
end
```

1. Instantiate an ActionTexter::Message

```
message = ActionTexter::Message.new(from: "phone number",
                          to: "phone number",
                          text: "message contents",
                          reference: "optional reference")
```

1. Deliver the message

```
message.deliver
```

1. **DO NOT** Instantiate the client and call deliver passing in the message. This breaks the beautiful abstraction
     of the client selector, and it also means observers and interceptors don't work

1. Set observers or interceptors

```
class MyInterceptor
    def delivering_sms(message)
        # Do something with the message. Modify its contents and return the modified object.
        # Or return nil / false to cancel the sending
    end
end

class MyObserver
    def delivered_sms(message, response)
        # Log, or do whatever you want with the fact that an SMS has been sent, and that's the response you got
    end
end

ActionTexter.register_interceptor(MyInterceptor.new)
ActionTexter.register_observer(MyObserver.new)
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
