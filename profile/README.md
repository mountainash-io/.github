# Mountainash

**Data logic written once, run wherever the data lives.**

[![Docs](https://img.shields.io/badge/Docs-docs.mountainash.io-2E7D32?style=flat-square&logo=materialformkdocs&logoColor=white)](https://docs.mountainash.io)
[![Website](https://img.shields.io/badge/Website-mountainash.io-555555?style=flat-square)](https://mountainash.io)
[![Python](https://img.shields.io/badge/Python-3.12%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://docs.mountainash.io/about/)
[![Status](https://img.shields.io/badge/Status-experimental-orange?style=flat-square)](#status)
[![License](https://img.shields.io/badge/License-Apache--2.0-blue?style=flat-square)](https://www.apache.org/licenses/LICENSE-2.0)

> **Experimental.** Every package is experimental and none is ready for production use. The six core packages are at version 0.1.0, nothing is published on PyPI yet, and APIs can change between versions. `main` is the release line; work happens on `develop`.

---

## Why Mountainash

Data work rarely stays in one place. A model starts on a Parquet extract on a laptop, reads reference files from S3 or an SFTP drop, and runs in production against a database. Each place brings its own client library, credentials and API, and the same logic tends to get rewritten for each one.

Mountainash gives that work one shape. **mountainash** holds the logic as Polars-style plans and compiles them for the engine that holds the data: native Polars, the libraries Narwhals reaches, and SQL engines through Ibis. The other packages supply what those plans need around them: configuration, credentials, storage, file formats and database connections. Each package has one job and composes with the others through its public API.

**Develop locally, run in production.** Build a model on Polars against a local extract, then point the same function at the full table in a database. Promotion changes the input, and the logic you tested is the logic that runs. Connection details live in configuration, so moving between development, staging and production changes files, not code.

**Many sources, one plan.** A single plan can join a database table, a CSV read from local disk and plain Python data. mountainash moves each operand to where the work runs and reports every move. Behind it, mountainash-transport reaches local disk, S3-compatible stores, SFTP and HTTP(S) through one API, and mountainash-files reads the formats.

---

## Packages

### Core ecosystem

| Package | What it does | Docs |
|---|---|---|
| [mountainash](https://github.com/mountainash-io/mountainash) | Polars-style expressions and relations that compile to native Polars, to the libraries Narwhals reaches and to SQL engines through Ibis, with schemas, conformance, data contracts and validation on the same plans. | [Book](https://docs.mountainash.io/mountainash/) |
| [mountainash-data](https://github.com/mountainash-io/mountainash-data) | Database connections from configuration across 20 Ibis dialects, tables handed to mountainash, and portable writes back under transactions. | [Book](https://docs.mountainash.io/mountainash-data/) |
| [mountainash-settings](https://github.com/mountainash-io/mountainash-settings) | Typed configuration on Pydantic v2: files, environment variables and secret stores merged in a predictable order, plus connection profiles that render driver arguments. | [Book](https://docs.mountainash.io/mountainash-settings/) |
| [mountainash-auth-client](https://github.com/mountainash-io/mountainash-auth-client) | Credential profiles rendered for HTTP, Boto or Paramiko clients, and OAuth2 and OAuth1 token lifecycles with an application-selected token store. | [Book](https://docs.mountainash.io/mountainash-auth-client/) |
| [mountainash-transport](https://github.com/mountainash-io/mountainash-transport) | One storage API for local disk, S3-compatible stores, SFTP and HTTP(S). The path picks the backend, with location policies, stream transforms and cross-store copy. | [Book](https://docs.mountainash.io/mountainash-transport/) |
| [mountainash-files](https://github.com/mountainash-io/mountainash-files) | CSV, TSV, JSON and NDJSON, Parquet, Excel and XML through one API, on any store mountainash-transport reaches, returning Arrow tables or Python records. | [Book](https://docs.mountainash.io/mountainash-files/) |

### Also in development

Built on mountainash, and not yet part of the ecosystem docs.

| Package | What it does | Docs |
|---|---|---|
| [mountainash-rules](https://github.com/mountainash-io/mountainash-rules) | Metadata-driven business rules. Rules are rows, not code: decision tables with multiple match strategies, ranked by specificity, evaluated in one vectorised pass, with per-rule explainability. | [Book](https://docs.mountainash.io/mountainash-rules/) |
| [mountainash-structuremap](https://github.com/mountainash-io/mountainash-structuremap) | Declarative, bidirectional mapping between hierarchical documents (XML, Pydantic and dataclass models) and typed relational tables. | Coming |

---

## How they fit together

```
                      mountainash
                 (expressions and plans)
                  /                   \
       mountainash-data          mountainash-files
         (databases)                  (formats)
              |                           |
              |                   mountainash-transport
              |                        (storage)
               \                         /
                 mountainash-auth-client
                      (credentials)
                            |
                   mountainash-settings
                     (configuration)
```

Each package depends only on the packages beneath it, so you can adopt mountainash-settings alone or add layers as a project needs them. [Architecture](https://docs.mountainash.io/architecture/) describes the layers in detail.

---

## Where to start

| If you are a… | Start with | Then add |
|---|---|---|
| **Data analyst** | mountainash, mountainash-data | |
| **Data engineer** | mountainash, mountainash-data, mountainash-settings | mountainash-transport and mountainash-files when inputs are files on local disk, S3, SFTP or HTTP |
| **Platform or infrastructure team** | mountainash-settings, mountainash-auth-client | mountainash-data, mountainash-transport |
| **File ingestion developer** | mountainash-transport, mountainash-files | mountainash-auth-client for credentials |

New to the project? Read the [mountainash book](https://docs.mountainash.io/mountainash/) first: it introduces the expression and relation abstractions everything else uses. [Use cases](https://docs.mountainash.io/use-cases/) shows the packages working together.

---

## Design principles

1. **Write the logic once and compile it per backend.** One plan runs on Polars during development and in the database in production.
2. **Check what a target supports before running.** A plan that a backend cannot run fails before it starts.
3. **Keep configuration and credentials out of code.** Typed settings, `secret:` references that are safe to commit, and credential profiles rendered for each client.
4. **Each package owns one concern** and composes with the others through public APIs.
5. **Three-valued logic for real-world data.** Conditions evaluate to TRUE, FALSE or UNKNOWN, not only true or false.

More in [Design philosophy](https://docs.mountainash.io/philosophy/).

---

## Built on

[Polars](https://pola.rs) · [Narwhals](https://narwhals-dev.github.io/narwhals/) · [Ibis](https://ibis-project.org) · [Apache Arrow](https://arrow.apache.org) · [Frictionless Data](https://frictionlessdata.io) · [Pydantic](https://docs.pydantic.dev)

---

## Status

Every package is open source under the Apache License 2.0. The six core packages are at version 0.1.0 and require Python 3.12 or later. None is published on PyPI yet; each book's installation chapter shows how to install from a source checkout.

---

Maintained by [Nathaniel Ramm](https://github.com/discreteds) in Melbourne. Bug reports, feature requests and contributions are welcome as issues on the relevant repository, or get in touch by [email](mailto:nathaniel@mountainash.io) or on [LinkedIn](https://linkedin.com/in/nathaniel-ramm).
