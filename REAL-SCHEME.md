# The whole scheme — beyond crypto

**Not a KIP.** If Kaspa vanished, this file should still describe a business a human can run.

The desk’s one principle is “skip centralised stablecoins for dapps.” Translated out of crypto: **do not become the bank, do not hold the customer’s money, still get paid, still prove it.**

That scheme is older than blockchains.

---

## 1. Split three jobs that crypto glued together

| Job | What the human needs | What Kaspa can be | What to use if Kaspa is the wrong tool |
| --- | --- | --- | --- |
| **Unit of account** | Price the cup, the hour, the API call in a number the tax office knows | Not this. A covenant cannot peg $1. Grams are **work**, not rent. kUSD is **reserved**. | EUR / USD / local fiat. Always. |
| **Settlement** | Money actually moves | KAS on L1. Optional covenant lock. | SEPA Instant + EPC QR, cash, cards, BTC/BTCPay, bank transfer |
| **Receipt** | Proof for dispute, refund, accountant | Txid, 1 sompi “still” number, timeout claim | PDF/UBL invoice, IBAN remittance ref, card last4, Taler payment confirmation |

PegLab lost dollars **0–0** because it tried to merge all three into a toy token. Parker won **receipts** because 1 sompi is only a **ref**. Keep that split.

**Desk law:** the ticket is always `amount_fiat` + `settle_method` + `receipt_id`. KAS is a settle method. Grams are a settle method for *work*. Neither is the price of coffee.

---

## 2. Track 1 is a till, not a chain

[BTCPay Server](https://btcpayserver.org) already is “anyone hosts, desk keeps 0, QR, invoice, plugins.” It is Bitcoin-first. Altcoins that used to sit in core (ZEC, XMR) were moved to **plugins**. There is a **Stripe** plugin. There are plugins that take Naira and pay the merchant in BTC. That is the world: shops want **one box** that speaks many rails.

Do **not** rebuild BTCPay badly and call it Gramlane.

Do this instead:

1. **Invoice object** (software): seller, buyer optional, fiat amount, VAT if you must, due date, payment refs[].
2. **Payment refs[]** can be: EPC QR (IBAN), `kaspa:` URI, stillpay lock outpoint, “marked paid in cash.”
3. **Receipt**: PDF + CSV line. If they paid KAS, attach txid. If they paid SEPA, attach end-to-end ID.
4. **Host**: a binary / Docker / PWA of *that* host. Second machine, not this desk’s float.

If a Kaspa plugin for BTCPay is less work than a new superapp, **that is the commercial path.** Check the plugin builder before writing another Go server. This collab does not claim a Kaspa BTCPay plugin exists; Kaspapay (Kasplabs) was **archived Aug 2024**. Treat that as a warning: payment gateways die when they are a side project.

---

## 3. Europe already has “scan to pay” without a token

**EPC QR / GiroCode (EPC069-12)** puts IBAN, amount, and remittance in a QR. Banking apps fill the SEPA transfer. SEPA Instant is seconds, 24/7, euro, legal tender in the euro area.

If the desk is in Europe and the customer is in Europe, **this is the default settle rail for goods.** Kaspa QR is the extra box for people who already hold KAS. Shipping only the Kaspa QR is why shops bounce.

Same pattern elsewhere: Pix (Brazil), UPI (India), FPS (UK). Learn the local instant-pay QR. Do not invent a fourth one.

---

## 4. GNU Taler is the honest cousin

[GNU Taler](https://taler.net) says it in the manual: **a pure payment system, not a new cryptocurrency.** Merchant has a **bank account**. Payments in USD/EUR. Uses HTTP 402 in the design. Customer gets a payment confirmation; merchant gets tax records; refunds exist until wire deadline.

That is the scheme this desk keeps trying to grow out of a UTXO:

- prepaid tokens (they call coins; you call grams)
- 402 on a URL
- receipt the wallet can show again
- income visible on the merchant side

Difference that matters: Taler **wires fiat** to an IBAN. Gramlane wires **nothing** until someone sends KAS, and the jar is `ledger.json`. If you want “beyond crypto,” Taler’s merchant backend is the reading list, not Argent.

---

## 5. HTTP 402 without a religion

HTTP 402 has been reserved since the 1990s. The web mostly used **401 + accounts + Stripe**.

In 2025–2026, **x402** (Coinbase origin; x402 Foundation under Linux Foundation, cards and Stripe among members) put 402 on **stablecoins**, mostly. Public dashboards show huge *tx counts* and modest *USD*. Chainalysis noted meme traffic in the Base surge. Do not quote those numbers as “agent GDP.”

For this desk:

| Do | Don’t |
| --- | --- |
| Treat 402 as **“this call costs X, here is how to pay”** | Treat 402 as Kaspa-specific physics |
| Bind [elldeeone/kaspa-x402](https://github.com/elldeeone/kaspa-x402) for the Kaspa envelope (TN10 alpha) | Invent envelope #4 |
| Steal [k402](https://github.com/Kali123411/k402) lock/voucher | Call it x402 v2 or adopted KCC-0402 |
| Also accept **API key + prepaid fiat credit** (the thing that already bills agents) | Pretend agents will buy grams before they buy USDC |

AgenC (Solana + x402, LOCAL Telegram export) is a case study of billed agents **on someone else’s chain**. Steal the *job* (meter the call). Do not integrate their repos.

**Done when:** `curl` → 402 → pay (KAS **or** a boring prepaid key) → server **checks** the payment → 200. If it does not check, it is theatre.

Gramlane today: `X-Kaspa-Payment` accepted at HTTP layer only. stillpay: unsigned `stillpay-quote-v1`. Both are demos.

---

## 6. Names and “run locally” without `kns://`

The need: don’t paste a seed into a random website; don’t run untrusted JS in someone else’s cloud with keys.

Boring solutions that already work:

- signed GitHub release + SHA256 (you already do this for the silverc zip)
- PWA of **your** host
- Tailscale / WireGuard to your PC
- Docker compose

`kns://` (name locates, chain settles, machine runs) is a nice overlay. Official KNS is still **indexer FCFS inscriptions**. Consensus does not know `alice`. Uniqueness is not Nakamoto.

Ship: **download, checksum, run, pay.** Overlay later. `https://alice.kas.limo` leaks DNS; say so.

---

## 7. Law, tax, refunds (the unglamorous scheme)

A shop that cannot:

- export CSV for the accountant (Discord already wants this; coderofstuff/kaspa-transaction-report exists)
- refund
- show VAT / invoice number
- survive a chargeback conversation (“here is the receipt”)

…is not Track 1. It is a demo.

Consumer law does not care that GHOSTDAG is elegant. If you sell to humans in the EU, invoices and refunds are the product. Crypto settlement is an implementation detail.

**Fill is not a business** is the right *legal* instinct: don’t take custody, don’t be the PSP if you are not licensed. Selling **software** (like BTCPay) or publishing **public goods** (Track 0) are the two clean shapes. Mixing them with a public jar that people treat as a bank is how you become an accidental issuer.

---

## 8. What survives if Kaspa is a hobby

Build these so they work with **zero** KAS:

1. Invoice + EPC QR + “mark paid” — **[`desk/`](desk/)**
2. PDF receipt + CSV — print `/receipt`, download `/csv`
3. 402 middleware with a prepaid API key — `X-Prepaid-Key`; unverified Kaspa headers **403**
4. Local-run app with checksums — `go build`; loopback by default

Then add:

5. `kaspa:` QR as another ref
6. stillpay lock as another ref
7. WorkCredit consume() when it is a UTXO, not a JSON file

If 1–4 never ship, 5–7 are a costume.

---

## 9. Kill-if (same as crypto, restated)

- Speaking kUSD as live  
- Pricing goods only in grams  
- 402 that does not verify  
- `ledger.json` sold as L1  
- tPEG as money  
- Fourth 402 envelope  
- Seed paste  
- “We skip Circle” while silently becoming the bank for one popular host
