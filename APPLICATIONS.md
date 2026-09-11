# Applications — commercial and other

Ranked. **Live** = you can do it on Toccata with v1.0.0 if you respect the four holes. **Maybe** = needs software this desk has not proven (txids, verified 402). **No** = wrong tool or a lie.

Pins: 11 Sep 2026 freeze. Recheck.

---

## A. Live today (if you write the script carefully)

Own UTXO. `validateOutputState` + `require(value)`. No foreign `readInputState`. No `State[].split()` tuples. TN10 first.

| Use | Who pays | Why Kaspa | Catch |
| --- | --- | --- | --- |
| **Timeout escrow** | Buyer locks; seller claims; timeout reclaims | KIP-17 is live. stillpay `.sil` exists. Parker 1-sompi series on TN10 | This desk’s journal still empty. Broadcast gated. `.sil` does not yet lock output value — don’t skim principal for fees |
| **Simple vault** | You lock your own KAS | Spend rule is the product | Not a bank. Recovery UX is the hard part (Kassword already did a version — don’t impersonate) |
| **Assurance / deposit** | “I put KAS up; I get it back if X” | Public lock beats a spreadsheet | Define X in the script, not in Discord |
| **Controlled asset that is still KAS** | Issuer + holder | Covenant id (KIP-20) is the identity | Not KCC-20. Not a ticker. Don’t call it a token sale |
| **ZK check as a gate** | Someone pays to pass a proof | KIP-16 precompile is live | Circuit + UX is the work. elldeeone Groth16 builtin is a compiler feature, not a product |

These are **commercial** the moment a freelancer, a landlord, or a small shop uses the lock instead of “trust me.” They are not commercial while they are localhost.

---

## B. Maybe — commercial if the boring layer exists

The chain piece is small. The company is invoices, hosting, support, refunds.

| Use | Fit | What must exist first | Beyond-crypto twin |
| --- | --- | --- | --- |
| **POS / walk-up till** | Track 1. Shops take KAS *and* fiat | Dual QR (EPC + `kaspa:`). Fiat price. Mark-paid. CSV | Square / SumUp / BTCPay + GiroCode |
| **Online invoice** | Freelancers, tiny SaaS | PDF + payment refs[] + timeout escrow optional | InvoiceNinja, Billstride EPC QR |
| **API / agent meter** | “Skip USDC for *this* dapp’s fees” | 402 that **verifies**; prepaid grams as WorkCredit UTXO, not `ledger.json` | Stripe metering, API keys, x402-on-USDC (the thing that actually has volume) |
| **Message postage** | Anti-spam inclusion fee | Tiny KAS or gram burn **after** verify | Email postage, Hashcash, stamps |
| **Name-addressed local app** | kns-spec overlay | One name, no seed, checksummed runtime | Domain + HTTPS + signed release |
| **Self-hosted “BTCPay for Kaspa”** | The actual software company | Plugin or small host; backups; multi-rail | BTCPay itself |

**Honest commercial shape:** sell the **host + support**, never the float. BTCPay Vision is the analogue. If you cannot explain the refund, you cannot invoice a shop.

---

## C. Other uses (not a shop)

| Use | Honest |
| --- | --- |
| **Public receipt / timestamp** | 1 sompi as a still number. Education. Not money |
| **Classroom depeg** | peglab-stp. WILL DEPEG. Keep it labeled |
| **Darwin battletest** | gramlanepeglab. Sequencing vs a toy dollar |
| **Implementer kit** | kns-spec, wallet tables, proven txs on covenants.kaspa.com |
| **Map / pins** | kaspa-master-file. Track 0 public goods |
| **Research pointer** | vProgs, DAGKnight, Kurrent, MWEB-like — **read**, don’t ship |

---

## D. No — wrong tool or delusion

| Temptation | Why no |
| --- | --- |
| **Kaspa dollar / kUSD live** | No issuer, no reserves, KCC-20 Draft. Algorithmic peg refused by this desk already |
| **DEX / native DeFi** | Roadmap on kaspaexplained/status. Not live. Not this desk |
| **Payroll in grams** | Labour law + fiat. Till vision even says coffee cannot honestly be 50,000 grams |
| **Replace Circle globally** | x402 Foundation’s commercial path *is* stables + cards. You can skip Circle **on this path**. You cannot skip the dollar as unit of account |
| **Replace HTTPS / DNS with kns://** | Overlay. Indexer FCFS. Fine as a kit. Not a new web |
| **Clone Kassword / Portrait / KaChat / AgenC** | Other products. Pointers only |
| **L2 bridged USDC as “our” till** | Explicitly out of path |
| **Argent multi-actor shop** | No tag. Two leader/delegate rules unimplemented. One own-UTXO until then |
| **Mining pool / hashrate product** | Not this desk. Discord volume ≠ app design |

---

## E. Where the money would actually be (if any)

In order of “a stranger might pay you,” not “a README is poetic”:

1. **Support + install of a self-hosted till** (fiat invoice, Kaspa optional).  
2. **Escrow as a product** for people who already hold KAS (OTC, freelance, deposit).  
3. **Track 0 grants / public goods** (map, kns-spec, honest docs). Not a business; a fund.  
4. **Agent 402** only after verify — and even then you are competing with USDC x402, which is the incumbent *for this exact job*.

If (1) and (2) have no TN10 txids, there is no (4). Do not skip to “agent economy.”

---

## F. What I would build first (full authority, still real)

1. stillpay TN10 journal: **one** lock, **one** claim, **one** reclaim. Publish txids.  
2. Invoice PDF: € amount, EPC QR, `kaspa:` QR, receipt id. Mark paid by hand. **Done in [`desk/`](desk/)** (print the receipt page).  
3. 402 middleware that rejects unverified headers. **Done in `desk`.**  
4. Recompile Till/Gramlane contracts on silverc **v1.0.0** with value conservation.  
5. Only then: WorkCredit consume() as a UTXO.

`desk` is items 2–3. It still does not watch the chain or the bank. Everything else is a pointer or a classroom.
