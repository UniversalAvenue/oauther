# OAuther [![Build Status](https://travis-ci.org/lexmag/oauther.svg)](https://travis-ci.org/lexmag/oauther)

Library to authenticate with [OAuth 1.0](http://tools.ietf.org/html/rfc5849) protocol.

## Installation

Add OAuther as a dependency to your `mix.exs` file:

```elixir
defp deps do
  [{:oauther, "~> 1.0.1"}]
end
```

After you are done, run `mix deps.get` in your shell to fetch the dependencies.

## Usage

Example below shows the use of [hackney HTTP client](https://github.com/benoitc/hackney)
for interacting with Twitter API.
Protocol parameters are transmitted using the HTTP "Authorization" header field.

```elixir
creds = OAuther.credentials(consumer_key: "dpf43f3p2l4k3l03", consumer_secret: "kd94hf93k423kf44", token: "nnch734d00sl2jdk", token_secret: "pfkkdhi9sl3r4s00")
# => %OAuther.Credentials{consumer_key: "dpf43f3p2l4k3l03",
# consumer_secret: "kd94hf93k423kf44", method: :hmac_sha1,
# token: "nnch734d00sl2jdk", token_secret: "pfkkdhi9sl3r4s00"}
params = OAuther.sign("post", "https://api.twitter.com/1.1/statuses/lookup.json", [{"id", 485086311205048320}], creds)
# => [{"oauth_signature", "10ZSs6eeWP+IfzElF5xFX/wsqnY="},
# {"oauth_consumer_key", "dpf43f3p2l4k3l03"},
# {"oauth_nonce", "M0vwJncX7T2GQwGXM4zFEa1mvL9RReLtkwwcBvlxG0A="},
# {"oauth_signature_method", "HMAC-SHA1"}, {"oauth_timestamp", 1404500030},
# {"oauth_version", "1.0"}, {"oauth_token", "nnch734d00sl2jdk"},
# {"id", 485086311205048320}]
{header, req_params} = OAuther.header(params)
# => {{"Authorization",
# "OAuth oauth_signature=\"10ZSs6eeWP%2BIfzElF5xFX%2FwsqnY%3D\", oauth_consumer_key=\"dpf43f3p2l4k3l03\", oauth_nonce=\"M0vwJncX7T2GQwGXM4zFEa1mvL9RReLtkwwcBvlxG0A%3D\", oauth_signature_method=\"HMAC-SHA1\", oauth_timestamp=\"1404500030\", oauth_version=\"1.0\", oauth_token=\"nnch734d00sl2jdk\""},
# [{"id", 485086311205048320}]}
:hackney.post("https://api.twitter.com/1.1/statuses/lookup.json", [header], {:form, req_params})
# => {:ok, 200, [...], #Reference<0.0.0.837>}
```

## License

This software is licensed under [the ISC license](LICENSE).
