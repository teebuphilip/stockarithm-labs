# Adapter Contract

Adapters return a pandas DataFrame. They should fail closed by returning an
empty DataFrame instead of raising network or schema exceptions.

Required conventions:

- `date` column is always present when rows exist.
- The primary observed value is named consistently: `value`, `count`,
  `interest`, `throughput`, or another documented metric.
- Adapter functions should not contain trading rules.
- Adapter functions should not know about ranking, promotion, or portfolio
  state.

This makes adapters reusable as public data-ingestion primitives while keeping
the product's scoring engine private.
