# Nightly Sync

This repository is generated from an allowlist in the private StockArithm source
repo. Treat generated files as downstream artifacts.

The exporter:

1. Rewrites private package imports.
2. Copies only approved adapters and docs.
3. Writes sample/frozen data and toy examples.
4. Runs a denylist scan for obvious secret patterns.
5. Optionally commits and pushes this public repo.

Do not hand-edit generated files unless the change is meant to be overwritten.
