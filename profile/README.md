# Mountain Ash

**A composable Python data engineering ecosystem.** Expressions, rules, validation, schema conformance and pipelines are built once against a single expression algebra, then run on Polars, pandas, DuckDB and the other backends reachable through Ibis and Narwhals. The backend becomes a deployment decision, not a code decision.

> **Status: under active development, not production ready.** None of the packages are on PyPI yet, and APIs can change without notice. `main` is the release line; work happens on `develop`.

Documentation: **[docs.mountainash.io](https://docs.mountainash.io)**

## Core framework

The foundation the other packages build on.

| Package | Intent |
|---|---|
| [mountainash](https://github.com/mountainash-io/mountainash) | Cross-backend expressions and relational plans. Write `col()`, `lit()`, `when()` in Polars-style syntax and compile to the selected backend. |
| [mountainash-data](https://github.com/mountainash-io/mountainash-data) | One connection interface for analytical backends: relational databases through Ibis, and Iceberg catalogs through PyIceberg. |
| [mountainash-settings](https://github.com/mountainash-io/mountainash-settings) | Typed configuration: layered YAML, TOML, JSON and environment variables, template resolution and Pydantic integration. |

## Rules and logic

| Package | Intent |
|---|---|
| [mountainash-rules](https://github.com/mountainash-io/mountainash-rules) | Metadata-driven business rules engine. Rules are rows, not code: decision tables with multiple match strategies, ranked by specificity, evaluated in one vectorised pass, with per-rule explainability. |
| [mountainash-structuremap](https://github.com/mountainash-io/mountainash-structuremap) | Declarative, bidirectional transformation between hierarchical payloads (XML, JSON) and relational tables. |

## Utilities

| Package | Intent |
|---|---|
| [mountainash-transport](https://github.com/mountainash-io/mountainash-transport) | Storage abstraction with one interface across local and cloud object stores. |

## Where to start

Start with [mountainash](https://github.com/mountainash-io/mountainash) and its [manual](https://docs.mountainash.io/mountainash/). It introduces the expression and relation abstractions everything else uses.

---

Built in Melbourne. Website: [mountainash.io](https://mountainash.io)
