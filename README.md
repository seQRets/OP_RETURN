# OP_RETURN Message Builder

**[→ Open the live tool](https://seqrets.github.io/OP_RETURN/)**

A single-page tool for writing a permanent message to the Bitcoin blockchain. Type your text, watch it encode to hex in real time, then follow step-by-step instructions for either a Trezor hardware wallet or your own Bitcoin Core node.

One HTML file. No dependencies, no build step, no analytics, no network requests. Your message never leaves your browser.

---

## What it does

- **Live hex encoding** — your text converted to the hex format OP_RETURN actually carries, as you type
- **Byte meter** — a running count against the 80-byte standard relay limit, with a warning when you cross it (non-ASCII characters cost several bytes each, so the count is not the character count)
- **Two guided paths** — Trezor Suite (five clicks, no command line) or Bitcoin Core (four commands)
- **Pre-filled commands** — your hex is substituted directly into the `bitcoin-cli` / Core console commands, formatted for whichever one you're using, with a copy button on each
- **Light and dark themes** — follows your system by default, with a toggle that overrides it and remembers the choice
- **Plain-English explanations** of why anyone would use OP_RETURN and what its limits are

## Why write to a blockchain?

An OP_RETURN output attaches a small piece of data to a Bitcoin transaction. Once confirmed, that data is copied to every full node on Earth, timestamped by a block nobody controls, and it stays there. Common uses:

- **Proof of existence** — publish a document's hash to prove the exact file existed before a given block, without revealing its contents
- **Notarization** — anchor contracts, audit logs, or records so anyone can verify them later, with no notary and no service that can disappear
- **Provenance** — commit certificates, credentials, or supply-chain events to a public, checkable history
- **Censorship resistance** — a statement in a block cannot be taken down, edited, or de-platformed
- **Protocol markers** — asset issuance, sidechain pegs, and layer-two commitments all ride on OP_RETURN
- **Permanence** — memorials, dedications, and messages meant to outlive any website

## Limits worth knowing before you broadcast

| | |
|---|---|
| **Size** | 80 bytes is the safe ceiling — roughly one sentence. Bitcoin Core 30 relaxed its default relay policy to allow larger payloads, but nodes on older versions or custom limits may not propagate them, so anything above 80 bytes is best-effort. Trezor Suite enforces 80 bytes strictly. |
| **Permanent** | There is no delete. Once confirmed it is on every node forever, and cannot be edited or retracted by anyone. |
| **Public** | It is not encrypted. Anyone can read it in a block explorer, permanently linked to the transaction that carried it. Never write anything private or identifying. |
| **Storage** | It stores a fingerprint, not a file. You cannot put an image or PDF on-chain this way — publish a hash and keep the file elsewhere. |
| **Value** | The output is provably unspendable and carries zero bitcoin. Coins sent to it are destroyed. Your funds ride in the transaction's other outputs. |
| **Policy** | Standard relay policy accepts one OP_RETURN output per transaction, and you pay the ordinary miner fee for the bytes. |

## Running it locally

No server required — it's a static file:

```bash
git clone https://github.com/seQRets/OP_RETURN.git
open OP_RETURN/index.html
```

Or serve it if you prefer:

```bash
python3 -m http.server 8000
```

The whole tool is `index.html` — one file, nothing else to install.

## Privacy

The page makes zero network requests. There are no fonts, scripts, trackers, or analytics loaded from anywhere. Everything — the encoding, the byte counting, the command generation — runs locally in your browser. You can verify this by opening your browser's network tab, or by reading the file: it's one self-contained document.

The only outbound links are the two in the footer, and you have to click them.

## Contributing

Issues and pull requests are welcome. Keep it a single dependency-free file — that constraint is the point.

## Support

If this saved you some time, tips are appreciated: **[coinos.io/seqrets](https://coinos.io/seqrets)**

## License

[MIT](LICENSE)

---

Built and maintained by **Toothjockey LLC**.
