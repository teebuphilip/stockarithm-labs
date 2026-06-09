# StockArithm Labs

Public adapter lab for StockArithm.

StockArithm is a live paper-traded signal lab. The private product scores,
ranks, and tracks market rules. This public repo shows the layer before that:
how messy public data sources get normalized into boring, inspectable signal
inputs.

The design boundary is intentional:

> Public data discipline. Private signal execution.

This is not an open-source trading engine. It is the public lab shelf: adapter
contracts, frozen samples, schemas, and toy examples that make the inputs easy
to inspect without exposing the scoring engine.

## Why This Exists

Most market-signal products ask for trust too early. They show a result, a
chart, or a confident narrative, then hide the input shape and the process that
produced it.

This repo takes the opposite side of that trade:

- Show the data-ingestion boundary.
- Keep every adapter small and boring.
- Normalize outputs into predictable schemas.
- Use frozen samples so examples are repeatable.
- Keep trading rules out of the public adapter layer.

The goal is not to prove that any single signal works. The goal is to make the
input side legible before StockArithm turns those inputs into private rankings,
paper trades, and paid dashboards.

## What You Can Inspect

- Public-safe adapters in `stockarithm_labs/adapters/`
- Adapter routing helper in `stockarithm_labs/adapter_router.py`
- Output schemas in `schemas/`
- Frozen sample data in `samples/`
- Toy examples in `examples/`
- Adapter contract notes in `docs/ADAPTER_CONTRACT.md`
- Nightly export notes in `docs/NIGHTLY_SYNC.md`

## What Stays Private

- Strategy scoring and ranking logic
- Algo registry internals
- Live portfolio state
- Promotion rules
- Paid signal detail
- Private dashboards and ledgers
- Any DBB2 code or archetype logic

## Quickstart

Run the toy example against frozen sample data:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python examples/toy_signal_from_sample.py
```

That example does not need API keys or live network calls. It is meant to show
the shape of a normalized signal input, not to produce a trade recommendation.

## Optional Live Adapters

Some adapters call public APIs. Those may require optional keys:

```bash
export FRED_API_KEY="..."
export EIA_API_KEY="..."
export OPENCHARGEMAP_API_KEY="..."
```

Adapters fail closed where practical: if a key is missing, the network is down,
or a source changes shape, the adapter should return an empty DataFrame instead
of pretending the input is usable.

## Repo Map

```text
stockarithm_labs/
  adapters/              Public-safe data adapters
  adapter_router.py      Lightweight text-to-adapter helper
  cache.py               Small local cache helper

schemas/
  adapter_output.schema.json
  adapter_manifest.schema.json

samples/
  fred_series.csv
  google_trends_urgent_home_repair.csv
  tsa_throughput.csv

examples/
  toy_signal_from_sample.py
  fetch_google_trends.py

docs/
  ADAPTER_CONTRACT.md
  NIGHTLY_SYNC.md
```

## Adapter Contract

Adapters return a pandas DataFrame and avoid trading logic.

Expected behavior:

- Include a `date` column when rows exist.
- Use a clear primary metric column such as `value`, `count`, `interest`, or
  `throughput`.
- Return an empty DataFrame on missing keys, network failures, or source schema
  changes.
- Do not know about scoring, ranking, promotion, portfolios, or paywalls.

That separation keeps the public repo useful while keeping the product's edge
inside the private engine.

## Public/Private Boundary

```text
Public repo
  raw source adapter -> normalized DataFrame -> schema/frozen sample -> toy demo

Private StockArithm engine
  normalized inputs -> scoring -> ranking -> paper trading -> dashboards
```

If you are building a signal product, this is the pattern worth stealing: open
the boring input discipline, keep the market edge and execution layer private.

## Status

This repo is generated from an allowlist in the private StockArithm source repo.
Generated files may be refreshed by nightly sync.
