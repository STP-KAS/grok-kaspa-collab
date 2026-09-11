# Grok × Kaspa collab

**Not Kaspa core. Not a KIP. Not a dollar.** Public notes by [@StppStp](https://x.com/StppStp) / [STP-KAS](https://github.com/STP-KAS), after the 11 Sep 2026 master-file freeze and grok heavy review.

This repo is the **collab page**: what Kaspa actually is, where it is useful, where this desk is still delusional, and what to do next. Pins live in [kaspa-master-file](https://github.com/STP-KAS/kaspa-master-file). Orders live in [THINK-BIG.md](https://github.com/STP-KAS/kaspa-master-file/blob/main/THINK-BIG.md). This file is the **honest read**.

**Built:** the till now has its own GitHub — **[STP-KAS/xai-reasoning-3](https://github.com/STP-KAS/xai-reasoning-3)** (xAI reasoning 3 + the binary). Copy in [`desk/`](desk/) is the same code. Price in **EUR**. SEPA (EPC) QR + optional `kaspa:` QR. You mark paid. HTTP 402 **refuses** unverified `X-Kaspa-Payment`. Does not hold funds.

```powershell
cd desk
go test ./...
go build -o desk.exe .
$env:DESK_IBAN="DE89370400440532013000"  # yours, not this example
.\desk.exe
```

http://127.0.0.1:8091

| Also in this repo | What |
| --- | --- |
| [desk/](desk/) | The till. Run it. |
| [REAL-SCHEME.md](REAL-SCHEME.md) | The whole scheme **beyond crypto**: invoices, SEPA QR, BTCPay, GNU Taler, tax, 402 as billed HTTP |
| [APPLICATIONS.md](APPLICATIONS.md) | Commercial + other uses, ranked live / maybe / no |

---

## Summary (read this first)

**Kaspa the protocol** is Bitcoin-shaped money that keeps extra honest blocks instead of throwing them away. Proof of work. Fair launch. About **10 blocks per second** on mainnet. Since Toccata (~30 Jun 2026), a UTXO can carry a **spend rule** (covenant). That is live. Wallets and shops are not.

**Kaspa is not** a dollar, a bank, an EVM, DAGKnight (still Proposed), vProgs (still research), or “instant irreversible payments.” SilverScript **v1.0.0** is a **compiler tag**, not an audited app store.

**This desk’s idea** is: skip Circle for dapp *fees* by prepaying **work** (grams = KIP-21 mass), not by minting kUSD. Goods that need a dollar stay on a **reserved till** that still settles in KAS. Anyone hosts. Desk keeps 0. Fill is not a business.

**What is true today**

- You can send KAS. You can lock KAS in a vault/escrow script.
- Mainnet covenants exist (kaspaexplained Sep 1 baseline, still the live page 11 Sep: **84,196** ever, **687** active, **~1.56M KAS**). TN10 still does more activity than mainnet.
- Parker already journaled **1-sompi receipts** on TN10. This desk’s [stillpay-tn10](https://github.com/STP-KAS/stillpay-tn10) is **ENGINE_SPEC**: broadcast off, no wasm submitter, **our** lock/claim/reclaim journal still empty.

**What is not true today**

- Grams on `ledger.json` are **one computer’s book**, not Nakamoto.
- HTTP 402 in the local dApps accepts a payment header **at HTTP layer only** — not verified on-chain (Gramlane HOW-IT-WORKS). stillpay’s quote is **unsigned**.
- Kaspa Till’s `kUSD` is **reserved, not live**. Merchant rate is a sign on the counter, not an oracle.
- Foreign `readInputState` is still unsafe (silverscript #234 closed unmerged). `State[].split()` tuples are broken on v1.0.0 (#249/#250). Amount is **not** locked by `validateOutputState`.

**The real scheme** is older than Kaspa: **price in money people understand (EUR/USD), settle on a rail you choose (SEPA, cash, BTC, KAS), hand over a receipt an accountant can file.** Crypto is one settle box. If Kaspa died tomorrow, the till/invoice/402 software should still make sense. That is [REAL-SCHEME.md](REAL-SCHEME.md).

---

## 1. Core idea

### Protocol (this is Kaspa)

Miners spend energy. There is no staking. Blocks can be found in parallel; **GHOSTDAG** keeps the honest ones and puts them in order. Fair launch, no premine.

Toccata (KIPs 16 / 17 / 20 / 21 **Active**) added spend rules:

| KIP | What it is |
| --- | --- |
| 16 | ZK precompile |
| 17 | Covenants (scripts that constrain how a UTXO can be spent) |
| 20 | Covenant IDs (the outpoint is the identity) |
| 21 | Lanes + grams of mass. **Not** a token named GRAM |

A covenant can say: this coin only moves if the next state matches, the timeout has passed, two keys signed, the amount is conserved. It **cannot** say: this coin is one US dollar.

Yonatan’s filter (from the intel pack): digital cash first; cohesive tooling; core ≠ product; based settlement. Sutton’s filter: **one** L1 covenant app **now**; multi-program vProgs **later**.

### This desk (this is the delusion, named on purpose)

One principle: **skip centralised stablecoins for dapps.**

Two different wants got glued together in the wider market:

1. **Boring fees** — a job should not jump in price when KAS/USD moves.
2. **A unit of account for goods** — a cup is €3, not “47,000 grams.”

Covenants can do (1) as **prepaid work**. They cannot mint (2). So the stack split:

| App | Job | Honest status |
| --- | --- | --- |
| **Gramlane** | Invoice **work** in grams | Local sequencer + jar book. WorkCredit.sil intended. Consume() not the live ledger yet. |
| **Kaspa Till** | Shelf in reserved `kUSD` | Asset **not live**. Settle KAS at a typed rate. |
| **KNS / kns-spec** | Name locates; Kaspa settles; machine runs | Official KNS = inscriptions + indexer. Overlay is this desk. Not a new chain. |
| **stillpay** | 1 sompi receipt + timeout escrow | Spec + local 402. **Our** TN10 journal empty. Parker’s is not. |

Fill: any Kaspa wallet (QR / `kaspa:` URI / paste txid). Inject = Kasware or Kastle only. This desk keeps **0**. Many hosts = many jars. That is **not** Circle. It is also **not** one global gram ledger until `consume()` is a real UTXO.

---

## 2. Vision (three layers — do not flatten)

**Layer A — protocol, 10 years.** PoW cash with spend rules. After the remaining ~1B KAS (~96% already out at the freeze: ~27.694B / ~28.704B), miners live on **fees**. That is the network’s problem. This desk does not “feed miners” by writing READMEs.

**Layer B — desk, 1–3 years.** Self-hosted till + prepaid work meter + names. Track 0 public goods (this map, kns-spec). Track 1 BTCPay-shaped software (you run it; we do not hold the float).

**Layer C — the lie to refuse.** A Kaspa dollar. Native DeFi. “We replaced Circle.” Argent ICC before a tag. KCC-20 named GRAM. Foreign `readInputState`. Fourth 402 envelope. Seed paste. TN12.

If Layer B never ships TN10 txids, Layer A is still Kaspa and Layer C is still a lie. The failure mode is **fanfic with a compiler tag**.

---

## 3. Where I would use it (honest)

Use Kaspa where **a public, checkable lock** is the point, and the amount can be KAS (or 1 sompi as a receipt unit). Do not use it where the customer, the tax office, or the supplier thinks in euros and you have no conversion story.

| I would | Why |
| --- | --- |
| **Timeout escrow** (freelance, deposits, “pay now, reclaim if silent”) | Toccata actually does this. stillpay’s `.sil` exists. Parker proved 1 sompi receipts. |
| **Vault / assurance / “this KAS only moves if…”** | Live pattern. Kassword / kaspa-pqv are other people’s; point, don’t clone. |
| **Prepaid work for *your own* API** | Grams as a meter for jobs you run. Only after the 402 checks a **chain** txid or a WorkCredit UTXO — not a header you accept because it looks like one. |
| **A second QR on an invoice that already has IBAN** | Settlement option. Not the price. See REAL-SCHEME. |
| **I would not** price coffee in grams or kUSD | Unit of account is fiat until a real L1 issuer exists (capital + law). PegLab **will depeg**. |
| **I would not** wait for this to replace cards, SEPA, or USDC in x402 | The 402 *industry* settled on stables (Coinbase origin; x402 Foundation). Kaspa’s binding is [elldeeone/kaspa-x402](https://github.com/elldeeone/kaspa-x402) **TN10 alpha**. Bind it. Do not pretend it is how Visa will clear. |

Full ranked list: [APPLICATIONS.md](APPLICATIONS.md).

---

## 4. What to do / improve / where to look

### Do (this week — same as THINK-BIG, not a new religion)

1. Recheck pins: silverc **v1.0.0** (`3ed9733`), rusty **v2.0.1**, `#250`, Argent tag-or-not, kccs#4/#14/#20, [kaspaexplained.com/status](https://kaspaexplained.com/status).
2. Put **this desk’s** TN10 txids in stillpay’s journal (lock / claim / reclaim), or keep saying the journal is empty.
3. Make 402 **verify** a txid or a UTXO. A header that is “accepted at HTTP layer only” is a demo, not a payment.
4. Recompile anything still on **v1-rc1** (Kaspa Till’s README still said that locally) onto **v1.0.0**. `require(value)` on every continuation.
5. Dual invoice: **EUR (or local fiat) + KAS due**. Never kUSD-as-money.
6. Clone [kaspa-x402](https://github.com/elldeeone/kaspa-x402). Do not start envelope #4. Steal k402’s lock; do not call it x402 v2 or adopted KCC-0402.

### Improve (the product, not the lore)

| Hole | Why it hurts | Fix |
| --- | --- | --- |
| Jar = `ledger.json` | One popular host is a Circle-shaped hole without the license | WorkCredit `consume()` on L1, or admit it is a tab |
| 402 not on-chain | Anyone can POST a fake header | Verify against node / indexer; then 200 |
| Timeout journal empty | ENGINE_SPEC is a story | stillpay-tn10 with `STILLPAY_BROADCAST=1` and a real wasm submitter — or keep broadcast off and stop talking mainnet |
| Amount not locked | Tutorial trap | `require(tx.outputs[i].value == …)` |
| `#234` | Hostile UTXO can slide field reads | Own-UTXO only. Never foreign `readInputState` |
| `#249` tuples | Compile error on the pin | `.0` / `.1` until `#250` merges |
| `#243` budget | Guess-by-rejection | Measure on TN10. Do not invent |
| Till kUSD | Looks like a peg | Label **reserved**. Rate is a sign. Or drop the ticker until an issuer exists |
| `kns://` vs HTTPS | Spec-heavy, seed-risk if sloppy | Ship signed binary + SHA256 first. Overlay second |
| Tax / refunds / chargebacks | Shops cannot use a toy | CSV export, PDF invoice, refund policy in software — boring, required |

### Where to look (do not wander)

**Law / live**

- https://kaspaexplained.com/status — live vs roadmap vs wrong  
- https://kaspaexplained.com/build-on-kaspa — covenant counts  
- https://docs.kaspa.org/toccata — official programmability  
- https://github.com/kaspanet/silverscript/releases/tag/v1.0.0 — compiler pin  
- https://github.com/kaspanet/rusty-kaspa/releases/tag/v2.0.1 — node pin  

**This desk**

- [kaspa-master-file](https://github.com/STP-KAS/kaspa-master-file)  
- [stillpay-tn10](https://github.com/STP-KAS/stillpay-tn10) / [stillpay-mainnet](https://github.com/STP-KAS/stillpay-mainnet) (mainnet repo exists; do not treat that as “we are live commerce”)  
- [peglab-poc](https://github.com/STP-KAS/peglab-poc) · [peglab-stp](https://github.com/STP-KAS/peglab-stp) (WILL DEPEG)  
- [kns-spec](https://github.com/STP-KAS/kns-spec)  
- [gramlane](https://github.com/STP-KAS/gramlane)  

**Steal, don’t worship**

- [elldeeone/kaspa-x402](https://github.com/elldeeone/kaspa-x402) — envelope to bind  
- [Kali123411/k402](https://github.com/Kali123411/k402) — lock/voucher to steal  
- [parker2017code/kaspa-explained](https://github.com/parker2017code/kaspa-explained) — 1 sompi receipt pack; `wTestUSD` cannot buy crops  
- [BTCPay Server](https://btcpayserver.org) — the actual Track 1 shape (self-hosted; plugins; even a Stripe plugin exists — that is the world)  
- [GNU Taler](https://taler.net) — payment system that **says it is not a cryptocurrency**; merchant has a bank account; HTTP 402 in the design docs  
- EPC QR / GiroCode (EPC069-12) — SEPA invoice QR. This is how a European shop already “anyone pays with a scan.”

**Out of path until tagged / Active**

Argent (no tag; two compiler rules unimplemented). KCC-0020 Draft. vProgs #139/#140/#142. DAGKnight KIP-2. Kurrent. MWEB-like forum 522. OpenSilver (not kaspanet, not always v1.0.0).

---

## 5. Pushback (keep it real)

- Naming it **project delusional** does not make the empty journal less empty.
- **Skip Circle** is a principle for *this path*. It is not a forecast that shops will stop wanting dollars. x402 volume in the wild is mostly stablecoins. Say that.
- **Anyone hosts** is real (BTCPay proved it for Bitcoin). **Many jars ≠ one gram** until L1 consume().
- **Compiler v1.0.0** is not a product. Four holes stay open. Do not sell the tag.
- **Fees after emission** is the network’s story. This desk creates fees only if someone actually pays for a thing.
- **AgenC** is a Solana marketplace. Steal billed-agent *work*. Do not integrate their stack.
- Local `127.0.0.1` is this machine. GitHub is not the running dApp.

Kill-if: foreign `readInputState` · tPEG as money · compiler tag sold as a dapp · fourth 402 · GRAM as KCC-20 · seed paste · TN12 · kUSD spoken as live.

---

*Freeze facts: 11 Sep 2026. Recheck DAA, PRs, and tags before quoting. Master file: https://github.com/STP-KAS/kaspa-master-file*
