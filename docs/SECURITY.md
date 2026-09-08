# Security policy

## Reporting a vulnerability

Email **quantum@11aiblockchain.com** with "SECURITY" in the subject line.

Please include what you found, how to reproduce it, and what you think the
impact is. A proof of concept helps and is not required.

11/11 AI is a small team, so the honest commitment is this: an acknowledgement
within five business days, and a plain answer about whether the issue is
confirmed, disputed, or already known. If a report goes unacknowledged past
that, assume it was missed rather than ignored and send it again.

Please do not open a public issue for a vulnerability before we have had a
chance to respond.

## Scope

In scope:

- This repository, the RFC-EG-0010 lineage verifier.
- The public verification endpoints on `control.11aiblockchain.com`,
  specifically `/.well-known/jwks.json`, `/v1/public/evidence`,
  `/v1/public/keys` and `/v1/public/proof-ledger`.
- The public verifier at
  [verify-11ai-proof](https://github.com/11-11AI/verify-11ai-proof), including
  any case where it reports a pass it did not actually perform. That class of
  bug is treated as a vulnerability here, not as a documentation defect.

Out of scope:

- Denial of service and volumetric testing against the live endpoints.
- Findings that require access to credentials or infrastructure we have not
  published.
- The marketing site.

## Known limitations, already stated

These are documented rather than hidden, and reports that restate them are
welcome but will not be treated as new findings:

- JWKS is served from the same origin as the evidence it authenticates, so a
  verifier can establish that a record is signed by the key that domain
  publishes, and not that the key belongs to 11/11 AI. Closing this requires
  publishing a key fingerprint through an independent channel.
- Record freshness is computed from a timestamp inside the signed evidence.
  It cannot be altered without breaking the chain, but it is the issuer's own
  clock, not a third-party time anchor.
- Evidence records minted before 5 September 2026 do not carry
  `ea11_state_hash`. Their evidence root cannot be recomputed by anyone,
  including 11/11 AI. This is reported per record rather than concealed.

## Cryptography

Report algorithm or construction concerns even if you cannot demonstrate an
exploit. The named chains are: EA-11 evidence is SHA-512; RFC-EG-0010 lineage
is SHA3-512 and BLAKE2b-512, dual and independently verified; SDK receipts are
SHA3-512 with Ed25519 signatures; the post-quantum envelope is ML-DSA-87
(FIPS 204) and SLH-DSA-SHA2-128f (FIPS 205).

A demonstration that any of those statements is false about the deployed
system is the single most useful report we can receive.
