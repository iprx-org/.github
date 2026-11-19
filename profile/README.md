# IPRX

**IPRX** is a worldwide IPv4 blocklist service built from honeypot observations. It tells you whether an address has been seen attacking public services, why it was listed or removed, and gives you a timestamped receipt you can check for yourself.

Website: [https://iprx.org](https://iprx.org) · Blocklist: [https://blocklist.iprx.org/](https://blocklist.iprx.org/) · Docs: [iprx-org/docs](https://github.com/iprx-org/docs)

## What you can use today

- **DNSBL** at [`bl.georgelambert.org`](https://github.com/iprx-org/docs/blob/main/usage.md#dns-blocklist). Reverse the octets of an IPv4 address and query that name. A listed address answers `A 127.0.0.2` (and a short TXT record). An address that is not listed returns NXDOMAIN.

  ```bash
  dig +short A 2.0.0.127.bl.georgelambert.org
  # 127.0.0.2
  ```

- **Public site** at [https://blocklist.iprx.org/](https://blocklist.iprx.org/): [About](https://blocklist.iprx.org/about), [Check](https://blocklist.iprx.org/check), [Nominate](https://blocklist.iprx.org/nominate), [Remove](https://blocklist.iprx.org/remove), and [History](https://blocklist.iprx.org/history) (by IP or IP hash). The apex [https://iprx.org/](https://iprx.org/) still serves the earlier IPRX homepage.

- **API** at [https://blocklist.iprx.org/api/v1/](https://blocklist.iprx.org/api/v1/) (also [https://iprx.org/api/v1/](https://iprx.org/api/v1/)). Interactive OpenAPI: [https://blocklist.iprx.org/docs](https://blocklist.iprx.org/docs). Public nomination and removal submissions use a honeypot field plus SHA-256 proof-of-work. Reserved and private addresses are rejected. Submissions are stamped, then wait for an operator decision. Nothing is listed or removed automatically. See the [API docs](https://github.com/iprx-org/docs/blob/main/api.md).

- **Receipts** as JSON, HTML, and PDF, from the web app or the API: `GET /api/v1/receipts/{event_id|hash_id}.json|.html|.pdf`, and a whole-IP bundle at `GET /api/v1/receipts/ip/{ip}.json|.html|.pdf`. Short URL: [https://iprx.org/r/{hash_id}.html](https://iprx.org/r/{hash_id}.html) (or `.json` / `.pdf`). The JSON is canonical and includes `canonical_sha256`. Receipts include the reason and, when known, PTR, ASN, and ISP detail. Each event is hashed and stamped on [Verae](https://verae.ai); the chain stores the digest, not the record itself. See [records and receipts](https://github.com/iprx-org/docs/blob/main/records-and-receipts.md).

## What is coming next

Downloadable block files will be published only after the lists are confirmed reliable.

- **Downloadable feeds**, including a plain IP list, optional coarse CIDRs, CSV with reasons, ipset and nftables loaders, a fail2ban list, a compact Bloom filter, and a one-packet exact prefix file. See [file formats](https://github.com/iprx-org/docs/blob/main/file-formats.md) and [update frequency](https://github.com/iprx-org/docs/blob/main/update-frequency.md).
- **Stamped manifests** for those GitHub block-file releases, so each upload can be checked against a Verae receipt. See [Nothing Up Our Sleeves](https://github.com/iprx-org/docs/blob/main/nothing-up-our-sleeves.md).

## Nothing Up Our Sleeves

Every timestamp, record, and published file in IPRX is meant to be provable, not trusted:

- **Timestamps.** Every listing, removal, request, and decision is anchored on Verae. The first stamp is permanent, and restamps only add history.
- **Data.** Each record's SHA-256 is on chain, so you can hash the record yourself and check it against the receipt.
- **GitHub uploads.** Each block-file release ships a manifest of file hashes. That manifest is stamped on Verae before it is pushed, and its receipt ID goes in the commit message.

Read the objectives and verification steps in [Nothing Up Our Sleeves](https://github.com/iprx-org/docs/blob/main/nothing-up-our-sleeves.md).

## Built on Verae

[Verae](https://verae.ai) (spelled V-E-R-A-E, pronounced *VEER-ay*, patent pending) is Verae’s own blockchain. IPRX uses it to timestamp list changes and individual events:

1. Build a structured event (who/what/why, or a feed manifest).
2. Hash it. Only the hash goes on chain.
3. A sharded packer batches digests, merklizes them, and commits the root in a block.
4. Anyone can verify a receipt against that root.

Measured on a six-shard test (November 2026): 5,000,000 digests were accepted at about 380,000 per second, and sealed at about 32,500 per second, with ~200 ms blocks and ~220 ms confirmations when the queue was empty. That is why a service like IPRX can attach a checkable receipt to every listing without waiting on a public third-party chain. More in [Verae’s Full Timestamped Tracking Blockchain](https://github.com/iprx-org/docs/blob/main/verae.md).

## Links

| | |
| --- | --- |
| Blocklist service | [https://blocklist.iprx.org/](https://blocklist.iprx.org/) |
| Apex site | [https://iprx.org](https://iprx.org) |
| API | [https://blocklist.iprx.org/api/v1/](https://blocklist.iprx.org/api/v1/) |
| Documentation | [https://github.com/iprx-org/docs](https://github.com/iprx-org/docs) |
| DNSBL | `bl.georgelambert.org` |
| Verae | [https://verae.ai](https://verae.ai) |
