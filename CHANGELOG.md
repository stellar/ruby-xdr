# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

### [3.0.2](https://www.github.com/astroband/ruby-xdr/compare/v3.0.1...v3.0.2) (2021-05-27)


### Bug Fixes

* make `XDR::Option` write empty strings as values ([#15](https://www.github.com/astroband/ruby-xdr/issues/15)) ([ccab229](https://www.github.com/astroband/ruby-xdr/commit/ccab22928a1104dee525211b51d1d567079d27ba))

## [Unreleased](https://github.com/astroband/ruby-xdr/compare/v3.0.2...master)

## [3.0.2](https://github.com/astroband/ruby-xdr/compare/v3.0.1...v3.0.2) - 2020-10-20
### Changed
- Relax lower bound for ActiveSupport dependency to support usage in Rails 4.2 ([#12](https://github.com/astroband/ruby-xdr/pull/12))

### Fixed
- Fix non-bang arm accessors for XDR::Union ([#13](https://github.com/astroband/ruby-xdr/pull/13))
- Fix .from_xdr encoding param to accept symbols ([#14](https://github.com/astroband/ruby-xdr/pull/14))

## [3.0.1](https://github.com/astroband/ruby-xdr/compare/v3.0.0...v3.0.1) - 2020-06-10
### Added
- Add encoding parameter to Union#to_xdr ([#7](https://github.com/astroband/ruby-xdr/pull/7)).

### Fixed
- Padding bytes are now properly validated when reading xdr values.  According to the XDR spec, padding must be zeros.

## [1.0.0](https://github.com/astroband/ruby-xdr/compare/v0.1.0...v1.0.0) - 2015-10-02
### Added
- the `to_xdr` helpers can take a second parameter that will encode the resulting output to hex or base64 when requested.
- the `from_xdr` helpers can take a second parameter that will trigger a decode from hex or base64 of the provided string before decoding from xdr.

### Changed
- `from_xdr` raises an ArgumentError when the input data is not fully consumed
