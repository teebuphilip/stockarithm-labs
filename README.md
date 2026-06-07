# StockArithm Labs

Public adapter lab for StockArithm.

This repo shows how messy public data sources become normalized signal inputs.
It does **not** include StockArithm scoring, ranking, promotion rules, live trade
logic, private state, or paid artifacts.

Generated from the private source repo at `2026-06-07T14:03:10Z`.

## What is included

- Public-safe adapters in `stockarithm_labs/adapters/`
- Adapter routing helper in `stockarithm_labs/adapter_router.py`
- Output schemas in `schemas/`
- Frozen sample data in `samples/`
- Toy examples in `examples/`

## What is intentionally excluded

- Strategy scoring and ranking logic
- Algo registry internals
- Live portfolio state
- Paid signal detail
- Private dashboards and ledgers
- Any DBB2 code or archetype logic

## Quickstart

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python examples/toy_signal_from_sample.py
```

Some adapters call public APIs. Those may require optional keys such as
`FRED_API_KEY`, `EIA_API_KEY`, or `OPENCHARGEMAP_API_KEY`.
