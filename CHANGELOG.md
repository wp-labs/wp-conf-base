# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.6.1] - 2026-09-19

### Changed

- 许可证由 Elastic License 2.0 改为 Apache License 2.0：此前本仓**没有** `LICENSE` 文件（`Cargo.toml` 与 README 却都声明 `Elastic-2.0`），本次一并补上标准 Apache-2.0 全文，并同步 `Cargo.toml` 的 `license` 字段与 README 的许可徽章/许可段

## [0.6.0] - 2026-09-19

### ⚠️ BREAKING CHANGES

- 依赖 `wp-model-core` 0.9 → 0.10（上游把整数类型正名：`Value::Digit` → `Value::Int`、`DataType::Digit` → `DataType::Int`、serde 名 `"digit"` → `"int"`、公开构造器 `from_digit` → `from_int`；`DataType::Array` 的载荷类型 `String` → `ArraySubtype`）
- 依赖 `wp-connector-api` 0.12 → 0.13（同步上游版本对齐；`wp-connector-api` 0.13 才依赖 `wp-model-core` 0.10，两者需同升）

### Changed

- README 补齐常用徽章（crates.io、crates.io 下载量、License、Rust Edition），CI 徽章由 `workflows/CI/badge.svg`（无链接）改用现代形式 `actions/workflow/status/.../ci.yml?branch=main` 并加上指向 Actions 的跳转链接；codecov 徽章保留

### Dependencies

- `wp-model-core`：`0.9` → `0.10`
- `wp-connector-api`：`0.12` → `0.13`

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

[Unreleased]: https://github.com/wp-labs/wp-conf-base/compare/v0.6.1...HEAD
[0.6.1]: https://github.com/wp-labs/wp-conf-base/compare/v0.6.0...v0.6.1
[0.6.0]: https://github.com/wp-labs/wp-conf-base/compare/v0.5.0...v0.6.0
[0.5.0]: https://github.com/wp-labs/wp-conf-base/compare/v0.4.0...v0.5.0
[0.4.0]: https://github.com/wp-labs/-wp-conf-base/releases/tag/v0.4.0
