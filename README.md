# Phel Minesweeper 💣

A terminal Minesweeper game written in [Phel](https://phel-lang.org/) — a functional Lisp that compiles to PHP.

Built as an exercise to stress-test Phel's onboarding docs and ergonomics for a developer coming from PHP and Clojure.

## Requirements

- PHP 8.3+
- [Composer](https://getcomposer.org/)

## Install

```bash
composer install
```

## Play

```bash
composer play
```

## Controls

```
r c         reveal cell at row r, col c
f r c       flag/unflag cell at row r, col c
q           quit
```

## Difficulty levels

| Level        | Grid  | Mines |
|--------------|-------|-------|
| Beginner     | 9×9   | 10    |
| Intermediate | 16×16 | 40    |
| Expert       | 30×16 | 99    |
| Custom       | any   | any   |

First click is always safe — mines are placed after your first reveal.

## Tests

```bash
composer test
```

54 tests covering board logic and input parsing.

## Architecture

The game follows a clean functional design with pure transforms on immutable data:

```
src/
  board.phel    — pure game logic (create, reveal, flag, flood-fill, win/loss checks)
  display.phel  — ANSI rendering (pure string transforms)
  input.phel    — command parser (pure)
  main.phel     — entry point and game loop (side effects isolated here)
tests/
  board-test.phel   — 41 tests for all board logic paths
  input-test.phel   — 13 tests for input parsing and edge cases
```

The board is represented as an immutable map: `{:width :height :mines :cells :state :flags}`.
Each cell is `{:mine? :revealed? :flagged? :adjacent}`.
Every action (reveal, flag) returns a new board — no mutation.

## Phel onboarding notes

Things that tripped me up, documented for posterity:

**`assoc` only takes one key-value pair** (unlike Clojure's variadic form).
Use `(-> m (assoc :k1 v1) (assoc :k2 v2))` or `(merge m {:k1 v1 :k2 v2})`.

**`vals` doesn't exist** — use `values`.

**`(is (not (pred val)))` has inverted semantics** in the test framework.
Use `(is (false? (pred val)))` instead when asserting falsy predicates.
[[See issue #1098 →](https://github.com/phel-lang/phel-lang/issues/1098)

**Lazy sequences from `filter`/`map` are not PHP arrays.**
After calling `(filter pred php-array)`, force evaluation with `(into [] ...)` before
using `peek`, array operations, or passing to PHP functions.

**`php/explode` returns a PHP array** — `filter` over it produces a lazy seq.
Wrap with `(into [] ...)` immediately if you need vector semantics. [See issue #1099 →](https://github.com/phel-lang/phel-lang/issues/1099)

## License

MIT
