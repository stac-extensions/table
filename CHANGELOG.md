# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Added `table:primary_datetime`
- Allow most fields also in the Asset Object

### Changed

- Allow more properties in the Column Object

### Deprecated

- `table:storage_options`: The property is not specific to tables but specific to fsspec. It should be generalized.
- `table:tables`: Tables in collections should be summarized using Item Asset Definitions or Collection Summaries instead.

### Removed

### Fixed

- Clarified usage of common metadata and extensions in the Column Object
- Improved schema

## [v1.2.0] - 2021-08-30

- Fixed version number in json schema.

## [v1.1.0] - 2021-08-30

### Fixed

- Fixed version number in json schema.

## [v1.0.1] - 2021-08-30

### Changed

- The `table:columns` field is no longer required on `Item`

## [v1.0.0] - 2021-08-27

Initial release.

[Unreleased]: <https://github.com/stac-extensions/table/compare/v1.0.0...HEAD>
[v1.2.0]: <https://github.com/stac-extensions/table/tree/v1.2.0>
[v1.1.0]: <https://github.com/stac-extensions/table/tree/v1.1.0>
[v1.0.1]: <https://github.com/stac-extensions/table/tree/v1.0.1>
[v1.0.0]: <https://github.com/stac-extensions/table/tree/v1.0.0>
