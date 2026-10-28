# Maintained dependency compatibility branch

This repository is a narrow fork of upstream `oauth2-rs` tag `5.0.0`, commit
`f3424b4b2190c83c6d031fdc71eed2351d49e0df`, maintained for Matrix MCP. It is not
an upstream release. Consumers must pin a reviewed full Git revision; publishing
to crates.io is disabled. The package version remains 5.0.0 for dependency
compatibility, while the Git revision identifies this fork.

The change upgrades SHA-2 to 0.11.1 (digest 0.11 / block-buffer 0.12.1) and the
example-only HMAC dependency to 0.13. It preserves the v5 public API, rand 0.8,
PKCE S256 outputs, HTTP-client features and OAuth behavior. The unused async-std
dev dependency is removed; examples import HMAC's renamed constructor trait.
Explicit elided lifetimes keep the declared compiler's warnings clean.

Upstream's SHA-2 proposal [PR352](https://github.com/ramosbugs/oauth2-rs/pull/352)
also changes random generation and public length parameter types; those changes
are deliberately excluded. Retire this fork when an upstream release removes the
advisory-affected chain and passes the same Matrix crypto/store qualification.
Do not suppress the advisory or infer safety from limited reachability.

## Fork compiler and verification

This fork requires **Rust 1.96**, the existing Matrix MCP compiler floor. The
committed Cargo.lock is the qualified dependency graph; the stale upstream
Rust 1.65 lockfile and upstream scheduled CI are replaced by the pinned Forge CI.

```sh
cargo +1.96.0 fmt --all --check
cargo +1.96.0 clippy --all-targets --all-features --locked -- -D warnings
cargo +1.96.0 test --all-features --locked
cargo +1.96.0 test --tests --examples --locked
cargo +1.96.0 test --doc --no-default-features --locked
cargo +1.96.0 test --tests --features curl --no-default-features --locked
cargo audit
osv-scanner scan source --lockfile Cargo.lock
```

The pre-existing two ignored doc examples need real external credentials; tests
do not authenticate to external providers. RFC 7636 Appendix B and independently
computed 43/64/65/128-character PKCE vectors guard hashing and encoding, including
block boundaries and the unchanged u32 random-length API. Consumer integration
must additionally prove Matrix's existing encrypted stores and derived keys.

---

# OAuth2

<a href="https://crates.io/crates/oauth2"><img src="https://img.shields.io/crates/v/oauth2.svg"></a>
[![Build Status](https://github.com/ramosbugs/oauth2-rs/actions/workflows/main.yml/badge.svg)](https://github.com/ramosbugs/oauth2-rs/actions/workflows/main.yml)

An extensible, strongly-typed implementation of OAuth2
([RFC 6749](https://tools.ietf.org/html/rfc6749)).

Documentation is available on [docs.rs](https://docs.rs/oauth2). Release notes are available on [GitHub](https://github.com/ramosbugs/oauth2-rs/releases).

For authentication (e.g., single sign-on or social login) purposes, consider using the
[`openidconnect`](https://github.com/ramosbugs/openidconnect-rs) crate, which is built on top of
this one.

## Upstream minimum supported Rust version

The upstream 5.0.0 release supported Rust 1.65. This maintained fork requires
Rust 1.96 as described above; it does not make upstream's older compiler promise.
