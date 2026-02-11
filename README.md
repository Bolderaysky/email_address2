# Crate email_address2

A Rust crate providing an implementation of an RFC-compliant `EmailAddress` newtype.
This crate is a maintained fork of [email_address](https://crates.io/crates/email_address).
The original crate appears to be no longer actively maintained, and this fork exists to 
continue development, add ecosystem integrations, and keep the crate usable in modern Rust projects.
The primary goal remains the same: provide a small, fast, and RFC-aware email address type 
suitable for validation and strongly-typed application code.

![MIT License](https://img.shields.io/badge/license-mit-118811.svg)
![Minimum Rust Version](https://img.shields.io/badge/Min%20Rust-1.40-green.svg)
[![crates.io](https://img.shields.io/crates/v/email_address.svg)](https://crates.io/crates/email_address2)
[![docs.rs](https://docs.rs/email_address2/badge.svg)](https://docs.rs/email_address2)
![Build](https://github.com/Bolderaysky/email_address2/workflows/Rust/badge.svg)
![Audit](https://github.com/Bolderaysky/email_address2/workflows/Security%20audit/badge.svg)
[![GitHub stars](https://img.shields.io/github/stars/Bolderaysky/email_address2.svg)](https://github.com/Bolderaysky/email_address2/stargazers)

## What's different from `email_address`
This fork currently provides:
- Continued maintenance and compatibility updates
- Optional SQLx support for database integration
- Dependency and tooling updates where necessary
- Compatibility improvements for modern Rust toolchains

The public API and behavior are intentionally kept as close as possible to the original crate to allow straightforward migration.
If you do not require SQLx integration or ongoing updates, the original crate may still be suitable for existing projects.

## Status

Currently, it supports all the RFC ASCII and UTF-8 character set rules as well
as quoted and unquoted local parts but does not yet support all the productions
required for SMTP headers; folding whitespace, comments, etc.

## Example

```rust
use email_address::*;

assert!(EmailAddress::is_valid("user.name+tag+sorting@example.com"));

assert_eq!(
    EmailAddress::from_str("Abc.example.com"),
    Error::MissingSeparator.into()
);
```

## SQLx Support
When the `sqlx` feature is enabled, `EmailAddress` integrates with SQLx so it can be stored and retrieved directly from supported database backends.

Example:
```toml
email_address2 = { version = "...", features = ["sqlx"] }
```

This allows EmailAddress to be used directly in query bindings and row decoding without manual conversion.

### Postgres
Additionally, `sqlx_postgres` feature can be enabled to allow using `EmailAddress` with postgres arrays.

## Specifications

1. RFC 1123: [_Requirements for Internet Hosts -- Application and Support_](https://tools.ietf.org/html/rfc1123),
   IETF,Oct 1989.
1. RFC 3629: [_UTF-8, a transformation format of ISO 10646_](https://tools.ietf.org/html/rfc3629),
   IETF, Nov 2003.
1. RFC 3696: [_Application Techniques for Checking and Transformation of
   Names_](https://tools.ietf.org/html/rfc3696), IETF, Feb 2004.
1. RFC 4291 [_IP Version 6 Addressing Architecture_](https://tools.ietf.org/html/rfc4291),
   IETF, Feb 2006.
1. RFC 5234: [_Augmented BNF for Syntax Specifications: ABNF_](https://tools.ietf.org/html/rfc5234),
   IETF, Jan 2008.
1. RFC 5321: [_Simple Mail Transfer Protocol_](https://tools.ietf.org/html/rfc5321),
   IETF, Oct 2008.
1. RFC 5322: [_Internet Message Format_](https://tools.ietf.org/html/rfc5322), I
   ETF, Oct 2008.
1. RFC 5890: [_Internationalized Domain Names for Applications (IDNA): Definitions
   and Document Framework_](https://tools.ietf.org/html/rfc5890), IETF, Aug 2010.
1. RFC 6531: [_SMTP Extension for Internationalized Email_](https://tools.ietf.org/html/rfc6531),
   IETF, Feb 2012
1. RFC 6532: [_Internationalized Email Headers_](https://tools.ietf.org/html/rfc6532),
   IETF, Feb 2012.

## TODO

1. Support comments.
1. Support line-feed and whitespace rules.
1. Does not parse _into_ `domain-literal` values, only does surface syntax check.
