# Proof of Enough

**Prove enough. Reveal nothing else.**

Track: Private Markets (ZECATHON)

Live demo: https://proof-of-enough.onrender.com
Video: [paste video link]

## The problem

A private market is only private until someone has to prove they can pay. Before an OTC trade, an auction or an escrowed deal, one side must show they can cover the position. Today that means exposing the whole portfolio or handing over a full viewing key, which reveals every transaction the wallet has ever made and can never be taken back.

## What it does

A counterparty asks one question: did this wallet hold at least X at block H? The holder sees exactly what the verifier will learn, approves it in their own browser, and signs a receipt that expires. The verifier checks the signature and expiry and learns one claim. The wallet stays closed.

## What runs

- Request, local approval, signed receipt, signature and expiry check
- Tamper test: changing the claim after signing makes the signature fail
- Refusal: the holder will not sign if its unspent notes at that height fall short
- A real Zcash testnet wallet (created with zcash-devtool): one note of 1 TAZ, mined at block 4539096, referenced by transaction id and output index

## What is simulated

- The transport between counterparty and holder happens inside one page (messages cross as serialized JSON)
- The wallet is read from a frozen export through the interface a local scanner will fill
- No viewing key is used or sent in this demo

## Limitations

This is scoped, expiring selective disclosure, not a zero-knowledge proof. A receipt is an attestation: the signature shows who asserted the claim, not that it is true. Receipts reference transactions, not note commitments yet. They do not show that the holder controls the notes, the amount held, or that the notes were unspent at that height, and they reveal which notes the holder points to. The signing key is not bound to an identity. A verifier who needs certainty without trusting anyone needs a zero-knowledge proof, and that is the next build.

## Run locally

```
git clone https://github.com/Bencoy09/proof-of-enough
cd proof-of-enough
python3 -m http.server 8000
```

Open http://localhost:8000. You can load your own wallet export in the holder pane: JSON with `network` and `notes`, where each note has `ref` (txid:output index), `value_zat`, `height`, `spent_height` (a block height, or null if unspent) and `pool`.

## Next

Zero-knowledge proof of the same claim, note commitments in receipts, automatic chain lookup, local wallet scanning, then shielded payments and a small SDK.
