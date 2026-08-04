# Reference

The public API lives at the module root (`github.com/go-ruby-toml/toml`). It is
**Ruby-shaped but Go-idiomatic**: names mirror Ruby's `toml`, while the surface follows
Go conventions — value types, explicit errors, no global state.

## Install

```sh
go get github.com/go-ruby-toml/toml
```

## Import

```go
import "github.com/go-ruby-toml/toml"
```

## API reference

The authoritative, always-current API reference is generated from the source by
pkg.go.dev:

- **[pkg.go.dev/github.com/go-ruby-toml/toml](https://pkg.go.dev/github.com/go-ruby-toml/toml)**

The module's [README](https://github.com/go-ruby-toml/toml#readme) carries worked
examples and the full, up-to-date surface. This page intentionally links to those
canonical sources rather than duplicating signatures that could drift out of date.

## Conformance

Behaviour is pinned by a **differential oracle** against reference Ruby: a corpus
is run through both the `ruby` binary and this library and the results are compared,
gated on the reference where relevant and skipping itself where `ruby` is absent so
the cross-arch lanes still validate the library.

In addition, the parser is measured against the canonical
[toml-lang/toml-test](https://github.com/toml-lang/toml-test) corpus for **TOML
v1.0.0**: it resolves **708 of 709 cases (99.86%)** to the reference verdict — all
210 valid cases parse and match, and 498 of 499 invalid cases are rejected.

Because the engine is **toml-rb-faithful** rather than a strict-spec validator, that
708/709 is its *faithful ceiling*, not a gap. The single case outside the count,
`invalid/string/multiline-quotes-01` (`a = """6 quotes: """"""`), is one that
toml-test marks invalid but that **`toml-rb` itself accepts** — its grammar consumes
the extra closing quotes (`"""""""""` ⇒ `"""`). This library matches `toml-rb`
there **by design**; diverging would make it *less* faithful to the gem it ports, so
the case is recorded as a known, intentional divergence rather than a failure.
(Conversely, where `toml-rb` is laxer than the spec — e.g. accepting `01` — this
library follows the spec and rejects the input.)
