# Changelog — `armature-payments`

All notable changes to this crate will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this crate adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Earlier changes are recorded in the workspace [`CHANGELOG.md`](../CHANGELOG.md).

## [Unreleased]

### Fixed

- **Breaking:** `Money::from_decimal`/`from_float` are deprecated and now panic rather than returning zero. An amount that did not round-trip through `i64` silently became a charge of nothing, reported as success; `try_from_decimal`/`try_from_float` are the fallible replacements.
- `Price::is_on_sale`/`discount_percent` check currency before comparing minor units.

### Changed

- Dependencies upgraded: `quick-xml` 0.42, `base64` 0.23, `tokio` 1.53, `uuid` 1.26, `rust_decimal` 1.43. The Braintree webhook XML parser now handles the separate entity-reference events that quick-xml 0.42 emits. Each text run is still trimmed as a whole and then unescaped, so `a &amp; b` keeps its spaces and unknown entities are still rejected.

## [0.3.1] - 2026-08-04

### Fixed

- Requirements on sibling armature crates name a minor instead of `0`. Under
  Cargo's 0.x rules `version = "0"` matches any release ever made, and edition
  2024 selects the MSRV-aware resolver, so a consumer declaring an older
  `rust-version` was handed the oldest version satisfying it — resolving
  `armature-core = "0"` on Rust 1.89 produced `armature-core 0.2.3` while an
  explicit `armature-core = "0.8"` elsewhere in the same graph pulled 0.8.2.
  Two copies of core, and a build failing on symbols the older one lacks. Each
  0.x minor in this family is a breaking change, so the requirement now names
  one. No API change.
