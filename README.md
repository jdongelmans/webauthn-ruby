# WebAuthn

Easily implement WebAuthn in your ruby web server

[![Gem](https://img.shields.io/gem/v/webauthn.svg?style=flat-square)](https://rubygems.org/gems/webauthn)
[![Travis](https://img.shields.io/travis/cedarcode/webauthn-ruby.svg?style=flat-square)](https://travis-ci.org/cedarcode/webauthn-ruby)

## Useful documentation on WebAuthn

- [WebAuthn article with Google IO 2018 talk](https://developers.google.com/web/updates/2018/05/webauthn)
- [Web Authentication API draft article by Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Web_Authentication_API)
- [W3C Draft Recommendation](https://w3c.github.io/webauthn/)

## User Agent compatibility

So far, the only browser that have web authentication support are:
  - Mozilla Firefox Quantum 60+ (Enabled by default).
  - Google Chrome 65+ (Disabled by default, go to chrome://flags to enable Web Authentication API feature). Note: it is enabled by default in 67+ as stated [here](https://www.chromestatus.com/feature/5669923372138496).

## Authenticator devises compatibility

The user agents mentioned in the previous section, only support USB FIDO2 or FIDO U2F enabled devises in their current implementations.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'webauthn'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install webauthn

## Usage

### Registration

#### Initiation phase

```ruby
credential_creation_options = WebAuthn.credential_creation_options

# Store the newly generated challenge somewhere so you can have it
# for the validation phase.
#
# You can read it from the resulting options:
credential_creation_options[:challenge]

# Send `credential_creation_options` to the browser, so that they can be used
# to call `navigator.credentials.create({ "publicKey": credentialCreationOptions })`
```

#### Validation phase

```ruby
attestation_object = "..." # As came from the browser
client_data_json = "..." # As came from the browser

attestation_response = WebAuthn::AuthenticatorAttestationResponse.new(
  attestation_object: attestation_object,
  client_data_json: client_data_json
)

# This value needs to match `window.location.origin` evaludated by
# the User Agent as part of the validation phase.
original_origin = "https://www.example.com"

if attestation_response.valid?(original_challenge, original_origin)
  # Register the new user
else
  # Handle error
end
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake` to run the tests and code-style checks. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/cedarcode/webauthn-ruby.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
