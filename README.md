# The Kaaval record

This repository is the published record of two projects. A publisher beside the engine
refreshes it every 15 minutes.

- `kaaval/ledger/` the signed, append-only ledger, one JSONL file per UTC day. Every
  decision, refusal, order, fill and equity mark the trading engine wrote.
- `kaaval/state/engine.json` where the engine stood at its last tick.
- `kaaval/proof/latest.json` the last run of the three proofs: verify, replay, attack.
- `kaaval/trades.csv` the trade log, rebuilt from the ledger.
- `kaaval/ledger-key.pub.hex` the public half of the signing key.
- `kaaval/manifest.json` and `vidiyal/manifest.json` what is in here right now.
- `vidiyal/reviews/` Vidiyal's review bundles: the same trades, graded after the fact.

Kaaval holds no real money. Every fill in this ledger is simulated against a Bitget order
book that was read at that instant and stored beside the fill.

## The signing key

```
41804536815325110d0feda7c57c9728730464b29a6b8637a458a71b95713a19
```

Ed25519. The private half stays on the machine that runs the engine and is never here.

## Verify it yourself

```bash
git clone <this repository> record
git clone <the kaaval repository> kaaval
cd kaaval
npm install
npm run verify:ledger -- ../record/kaaval/ledger ../record/kaaval/ledger-key.pub.hex
```

The check walks the hash chain, tests every signature against the public key above, and
names the first entry that does not fit. It then recomputes every fill from the order book
recorded beside it.

## What is never here

No private key, no API credential, no environment file. The publisher refuses to run at all
if a file that looks like any of those is sitting in a directory it copies from.
