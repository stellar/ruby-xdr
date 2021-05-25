# XDR, for Ruby

[![Build Status](https://travis-ci.org/stellar/ruby-xdr.svg?branch=master)](https://travis-ci.org/stellar/ruby-xdr)
[![Code Climate](https://codeclimate.com/github/stellar/ruby-xdr/badges/gpa.svg)](https://codeclimate.com/github/stellar/ruby-xdr)

XDR is an open data format, specified in [RFC 4506](http://tools.ietf.org/html/rfc4506.html).  This library provides a way to read and write XDR data from ruby.  It can read/write all of the primitive XDR types and also provides facilities to define readers for the compound XDR types (enums, structs and unions)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'xdr'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install xdr

## Usage

```ruby
# Reading/writing a primitive values
XDR::Bool.to_xdr(false)                # => "\x00\x00\x00\x00"
XDR::Bool.from_xdr("\x00\x00\x00\x01") # => true

# Reading/writing arrays
XDR::Array[XDR::Int,2].to_xdr([1,2]) # => "\x00\x00\x00\x01\x00\x00\x00\x02"
XDR::Array[XDR::Int,2].from_xdr("\x00\x00\x00\x03\x00\x00\x00\x04") # => [3,4]

# Defining an enum

class ResultType < XDR::Enum
  member :ok,       0
  member :error,    1
  seal
end

# Using enums

ResultType.ok == ResultType.error # => false
ResultType.to_xdr(ResultType.error) # => "\x00\x00\x00\x01"
ResultType.from_xdr("\x00\x00\x00\x00") # => ResultType.ok(0)

# Defining structs

class MessageWithHash < XDR::Struct
  attribute :message, XDR::String[]
  attribute :hash, XDR::Opaque[20]
end

# Using structs

s = MessageWithHash.new
s.message = "Hello world"
s.hash = Digest::SHA1.digest(s.message)
xdr = s.to_xdr # => "..."

s2 = MessageWithHash.from_xdr(xdr)

# Defining unions

class Result < XDR::Union
  switch_on ResultType, :type

  switch :ok
  switch :error, :message

  attribute :message, XDR::String[]
end

# Constructing unions
Result.ok
Result.error("You didn't say please")

# Using unions

Result.ok.to_xdr # => "\x00\x00\x00\x00"
Result.error("boom").to_xdr # => "\x00\x00\x00\x00\x00\x00\x00\x04boom"

Result.from_xdr("\x00\x00\x00\x00") # => #<Result ...>

```

## Thread safety

Code generated by `xdrgen`, which targets this library, uses autoload extensively.
Since autoloading is not thread-safe, neither is code generated from xdrgen. To
work around this, any module including `XDR::Namespace` can be forced to load
all of it's children by calling `load_all!` on the module.

## Code generation

ruby-xdr by itself does not have any ability to parse XDR IDL files and produce a parser for your custom
data types.  Instead, that is the responsibility of [xdrgen](http://github.com/stellar/xdrgen). `xdrgen` will
take your `.x` files and produce a set of ruby files that target this library to allow for your own custom types.

See [ruby-stellar-base](http://github.com/astroband/ruby-stellar-sdk/tree/master/base/generated) for an example.

## Contributing

1. Sign the [Contributor License Agreement](http://goo.gl/forms/6r7I4S0K4z)
2. Fork it ( https://github.com/astroband/ruby-xdr/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
