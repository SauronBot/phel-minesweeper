# Changelog

All notable changes to phel-minesweeper will be documented here.

## Unreleased

### Changed

- Upgraded `phel-lang/phel-lang` dependency to `dev-main` to use latest fixes
- **`src/board.phel`**: replace chained `(-> board (assoc :k v) (assoc :k2 v2))` with variadic `(assoc board :k v :k2 v2)` — uses [phel-lang#1102](https://github.com/phel-lang/phel-lang/pull/1102)
- **`src/input.phel`**: remove `(into [] ...)` workaround around `filter` before `peek` — uses [phel-lang#1101](https://github.com/phel-lang/phel-lang/pull/1101)
- **`src/main.phel`**: replace 4-line chained `assoc` with single variadic call
- **`tests/board-test.phel`**: replace `(is (false? x))` with `(is (not x))` — uses [phel-lang#1100](https://github.com/phel-lang/phel-lang/pull/1100)

## [0.1.0] — 2026-02-22

### Added

- Initial release: terminal Minesweeper written in Phel (Lisp → PHP)
- Beginner / Intermediate / Expert / Custom difficulty presets
- First-click safety (mines placed after first reveal)
- BFS flood-fill for empty cell expansion
- Flag support
- 54 tests covering board logic, input parsing, and display
