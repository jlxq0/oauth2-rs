# Scope and gates

Maintain only the dependency compatibility branch described in README.md. Keep
upstream licensing/attribution, the oauth2 v5 public API, random generation and
OAuth behavior. Pin consumers to a reviewed full revision; do not publish this
package to crates.io or deploy Matrix from this repository.

Run README verification on Rust 1.96 and fresh advisory data. CI has no publishing
or deployment step. Require independent exact-head review and successful actual
CI jobs before merging; a skipped job is not build evidence.

## Known Pitfalls

- 2026-10-29: Updating direct SHA-2 in Matrix leaves an older digest/block-buffer
  chain through oauth2. Check the complete reverse dependency graph; no advisory
  exception is permitted. Upstream PR352 also changes rand and public argument
  types, so do not import it wholesale into a compatibility fix.
- 2026-10-29: HMAC 0.13 moves new_from_slice to KeyInit. Compile all features and
  examples, not just the no-default Matrix dependency shape. Only an unused
  async-std development dependency was removed; do not silently omit tests.
- 2026-10-29: A lockfile for the old Rust 1.65 promise can retain obsolete
  dependencies. This fork declares/tests Rust 1.96 with one committed lockfile.
  Changes to that floor or the consumer's Matrix SDK/store format need their own
  qualification.
