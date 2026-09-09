# OP_RETURN Message Builder

**[→ Open the live tool](https://seqrets.github.io/op_return/)** &nbsp;·&nbsp; **[↓ Download it](https://seqrets.github.io/op_return/index.html)** (saves as `op_return.html`, works offline)

A single-page tool for writing a permanent message to the Bitcoin blockchain. Type your text, watch it encode to hex as you go, then follow step-by-step instructions for either a Trezor hardware wallet or your own Bitcoin Core node.

One HTML file. No dependencies, no build step, no analytics, no network requests. Your message never leaves your browser.

---

## What it does

- **Live hex encoding.** Your text converted to the hex that OP_RETURN actually carries, updating as you type.
- **Hex input mode.** Paste raw hex instead of text, for anchoring a file hash or any payload that is not readable text. Spacing, newlines, a `0x` prefix and either case are all accepted.
- **Paste safeguards.** Hex or base64 pasted into the text field would be published as those characters rather than the data they stand for: bigger, and not what you meant, while the command still looked correct. Both are now recognised in either field and converted in one click.
- **The byte count appears twice**, once under the message and again beside the funding command, so the size is on screen at the moment you copy rather than only at the top of the page.
- **Byte meter.** A running count against the 80-byte limit, with a warning when you cross it. Accented letters and emoji cost several bytes each, so this is not a character count: a plain emoji costs 4 bytes, one with a skin tone costs 8.
- **A guided flow.** Type a message, press Next, pick your wallet, follow the steps. The next action is always on screen.
- **Two paths.** Trezor Suite, about five clicks and no command line, or Bitcoin Core, four commands on your own node.
- **Pre-filled commands.** Your hex is substituted straight into the commands, reformatted for either the Bitcoin Core app console or a terminal, with a copy button on each.
- **Light and dark themes.** Light by default, with a toggle that remembers your choice.
- **A jump menu** to the explanation and limits sections.
- **Plain English throughout.** No relay policy, no UTXO set, no jargon left unexplained.

## Why write to a blockchain?

An OP_RETURN output attaches a small piece of data to a Bitcoin transaction. Once confirmed, that data is copied to every full node on Earth, timestamped by a block nobody controls, and it stays there. Common uses:

- **Proof of existence.** Publish a document's hash to prove the exact file existed before a given block, without revealing its contents.
- **Notarization.** Anchor contracts, audit logs, or records so anyone can verify them later, with no notary and no service that can disappear.
- **Provenance.** Commit certificates, credentials, or supply-chain events to a public, checkable history.
- **Censorship resistance.** A statement in a block cannot be taken down, edited, or de-platformed.
- **Protocol markers.** Asset issuance, sidechain pegs, and layer-two commitments all ride on OP_RETURN.
- **Permanence.** Memorials, dedications, and messages meant to outlive any website.

## Limits worth knowing before you broadcast

| | |
|---|---|
| **Size** | 80 bytes is the safe limit, roughly one sentence. Past it the Trezor path is switched off in the page, since Suite will not send it, and the tool points you at Bitcoin Core instead. You can go longer, but only by sending with Bitcoin Core v30 or newer, and even then some of the computers that make up the network may refuse to pass it along, with nothing to tell you it happened. Trezor Suite will not send more than 80 bytes at all. |
| **Permanent** | There is no delete. Once confirmed it is on every node forever, and nobody can edit or retract it. |
| **Public** | It is not encrypted. Anyone can read it in a block explorer, permanently linked to the transaction that carried it. Never write anything private or identifying. |
| **Storage** | It stores a fingerprint, not a file. You cannot put an image or a PDF on-chain this way. Publish a hash and keep the actual file elsewhere. |
| **Value** | Nothing can ever be spent from the message output. It carries zero bitcoin, no key can unlock it, and any coins sent there are destroyed. Your own funds ride in the transaction's other outputs. |
| **Policy** | Assume one message per transaction, since that is the rule older nodes enforce and therefore what travels reliably. Bitcoin Core v30 and newer allow several in a single transaction, but older nodes may not pass those on. You pay the ordinary miner fee for the bytes either way. |

## Sending more than 80 bytes

Larger payloads are a normal thing to want, not an error, and the page treats them that way. Above 80 bytes it stops describing the message as too long and simply states the requirement: Bitcoin Core v30 or newer.

The Trezor path is a different matter. Trezor Suite will not send more than 80 bytes at all, so above that limit the Trezor card is disabled and its steps are replaced with your byte count and a link through to the Bitcoin Core path. The trigger is size, not input mode: a 32-byte hash pasted as hex is well inside the limit, so that path stays available.

## Running it locally

Grab it straight from the footer of the live page, which saves it as `op_return.html` and runs offline with no further setup. Or clone it.

No server required, it is a static file:

```bash
git clone https://github.com/seQRets/op_return.git
```

```bash
open op_return/index.html
```

Or serve it if you prefer:

```bash
python3 -m http.server 8000
```

The whole tool is `index.html`. One file, nothing else to install.

## Privacy

The page makes zero network requests. No fonts, scripts, trackers, or analytics are loaded from anywhere, and the typefaces are whatever your system already has. Everything, the encoding, the byte counting, the command generation, runs locally in your browser. You can verify this by opening your browser's network tab, or by reading the file: it is one self-contained document.

The only outbound links are the three in the footer, and you have to click them.

## Tests

`test.html` loads `index.html` in a hidden frame and drives the real page, asserting against its actual DOM. It is not part of the tool and `index.html` does not know it exists, so the single-file constraint is untouched.

Serve the folder and open it. It will not work opened straight from disk, because a browser will not let a `file://` page read into its own frame:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000/test.html`. Green means every assertion passed.

It covers byte counting including emoji, hex normalisation and rejection, base64 detection in both fields, the double-encoding guards and their one-click fixes, the false-positive sweep over ordinary prose, the 80-byte Trezor gate, command formatting, and the promises this README makes about the default theme and zero network requests.

The suite was checked by breaking the code on purpose: reintroducing the lowercase-before-base64-detection bug turns it red on the exact assertion written for it.

## Contributing

Issues and pull requests are welcome. Keep `index.html` a single dependency-free file, that constraint is the point. If you change behaviour, add an assertion to `test.html`.

## Support

If this saved you some time, tips are appreciated: **[coinos.io/seqrets](https://coinos.io/seqrets)**

## License

[MIT](LICENSE)

---

Built and maintained by **Toothjockey LLC**.
