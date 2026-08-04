# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.5.0] - 2026-08-04

### ⚠️ BREAKING CHANGES

- 依赖 `wp-model-core` 0.8 → 0.9（上游新增 `Value::BigUint` / `DataType::BigInt` 变体）
- 依赖 `wp-connector-api` 0.10 → 0.12

### Dependencies

- `wp-model-core`：`0.8` → `0.9`
- `wp-connector-api`：`0.10` → `0.12`

## [0.4.0] - 2026-05-03

### Changed
- Bump orion-error from 0.7 to 0.8 (via orion_conf 0.6 → 0.7)
- Replace `StructError::from(reason)` with `reason.to_err()` in tests
- Update test assertions from `err.to_string()` to `err.reason()` pattern matching (0.8 Display no longer includes variant data)
- Fix `derive_more` v2.x feature gate

[Unreleased]: https://github.com/wp-labs/wp-conf-base/compare/v0.5.0...HEAD
[0.5.0]: https://github.com/wp-labs/wp-conf-base/compare/v0.4.0...v0.5.0
[0.4.0]: https://github.com/wp-labs/-wp-conf-base/releases/tag/v0.4.0
