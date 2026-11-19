# IPRX

**IPRX** is a worldwide IPv4 blocklist service built from honeypot observations. It tells you whether an address has been seen attacking public services, why it was listed or removed, and gives you a timestamped receipt you can check for yourself.

Website: [https://iprx.org](https://iprx.org) · Docs: [iprx-org/docs](https://github.com/iprx-org/docs)

## What you can use today

- **DNSBL** at [`bl.georgelambert.org`](https://github.com/iprx-org/docs/blob/main/usage.md#dns-blocklist). Reverse the octets of an IPv4 address and query that name. A listed address answers `A 127.0.0.2` (and a short TXT record). An address that is not listed returns NXDOMAIN.

  ```bash
  dig +short A 2.0.0.127.bl.georgelambert.org
  # 127.0.0.2
  ```

## What is coming next

These parts are implemented in the generator and record format, but they are not a public product yet. Downloadable block files will be published only after the lists are confirmed reliable.

- **Downloadable feeds**, including a plain IP list, optional coarse CIDRs, CSV with reasons, ipset and nftables loaders, a fail2ban list, a compact Bloom filter, and a one-packet exact prefix file. See [file formats](https://github.com/iprx-org/docs/blob/main/file-formats.md) and [update frequency](https://github.com/iprx-org/docs/blob/main/update-frequency.md).
- **Per-IP receipts** on [Verae](https://verae.ai), George’s timestamped tracking blockchain. Each add, remove, or restamp is hashed (the chain stores the digest, not the record itself) and can be proved later. Receipts include the reason, and later records can add PTR, ASN, and ISP detail. JSON, HTML, and PDF downloads are [coming soon](https://github.com/iprx-org/docs/blob/main/api.md#download-a-receipt-coming-soon). See [records and receipts](https://github.com/iprx-org/docs/blob/main/records-and-receipts.md).
- **Nomination and removal requests** — a web form and API on [iprx.org](https://iprx.org). Submissions are stamped, then wait for an operator decision. Nothing is listed or removed automatically. See the [API](https://github.com/iprx-org/docs/blob/main/api.md).

To check an IP or look up history by IP, IP hash, hostname, or ISP, use [iprx.org](https://iprx.org) once those pages are live.

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
| Service | [https://iprx.org](https://iprx.org) |
| Documentation | [https://github.com/iprx-org/docs](https://github.com/iprx-org/docs) |
| DNSBL | `bl.georgelambert.org` |
| Verae | [https://verae.ai](https://verae.ai) |
