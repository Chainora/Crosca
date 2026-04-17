# Crosca

**Trustless rotating savings for Vietnam's 10M+ hụi players — built on Initia.**

---

## Overview

Hụi (rotating savings clubs) is Vietnam's oldest community finance tradition.
Over 10 million people participate, circulating an estimated $2B+ per year —
entirely informally, with zero consumer protection.

crosca brings hụi on-chain. Smart contracts replace the trusted middleman.
On-chain reputation replaces blind faith. Social vouching preserves the human
relationships that make hụi work.

> Blockchain handles the money. People handle each other.
> crosca handles both.

---

## Problem

**1. Default risk with no consequences**
When a member receives the pot and disappears, victims have no legal recourse.
Defaulters leave no financial trace and can join new groups immediately.

**2. The organiser holds all power — unchecked**
The hụi chủ (organiser) collects, holds, and distributes all money manually —
via notebooks or Zalo. No member can independently verify anything.

**3. No portable credit history**
A member with 10 years of perfect repayment has nothing to show for it when
joining a new group. Trust resets to zero every time.

**4. Existing tech makes it worse**
Web2 apps digitise the process but don't change the risk structure. Money still
flows through one person. Defaults still happen — just faster.

---

## Solution

crosca is a smart contract protocol that separates two things hụi has always
bundled together: **trusting the money** and **trusting the people**.

### Smart Contract Escrow
All contributions go directly from member wallets to a smart contract on Initia.
No one — not even the organiser — can touch the funds outside the hụi process.
Organiser-absconding is technically impossible.

### Collateral System
Each member locks collateral proportional to their actual remaining risk:
Collateral is released gradually each period they pay on time.
It earns yield while locked — so honest players get their money back plus interest.

### Social Vouching
An existing member can vouch for a new member, reducing their collateral by 50%.
The voucher takes on 30–50% of liability if the vouchee defaults intentionally.
This encodes the social accountability mechanism hụi has always relied on —
with real financial consequences attached.

### On-Chain Reputation
Every on-time payment is recorded permanently on-chain.

Reputation reduces collateral requirements across all future groups.
It cannot be deleted, reset, or transferred — it follows the person's KYC identity.

### Two-Track Member Approval
- **Fast track**: Vouched by existing member → approved immediately
- **Group vote**: No voucher → 2/3 approval required within 48h (silence = consent)

### Default Resolution (Fully Automated)
When a member misses payment:
1. 72h grace period with push notifications
2. If still unpaid → auto-deduct from their collateral
3. If collateral insufficient → deduct from voucher's stake
4. If still insufficient → reserve fund covers the remainder

The group always receives full payment. No human intervention required.

---

## How It Works

---

## Initia Integration

crosca is built natively for Initia — not a generic EVM deployment.

- **Appchain**: Dedicated execution environment for hụi transactions
- **OPinit**: Batch micro-transactions (weekly contributions) into single settlements
- **Move VM**: Type-safe asset management, immune to reentrancy attacks
- **IBC / Enshrined Liquidity**: Members pay in USDT/USDC from any connected chain
- **Initia Oracle**: Real-time USDT/VND rate for Vietnam-friendly UI
- **Native Staking**: Collateral pool auto-staked to generate yield for users and protocol

---

## Revenue Model

| Source | Rate | Who pays |
|---|---|---|
| Platform fee | 1% per pot | Deducted automatically at distribution |
| Yield spread | 50% of collateral yield | Protocol share of staking rewards |
| Premium features | Monthly subscription | Organisers who need advanced tools |

No hidden fees. All rates displayed before joining a group.

---

## Economic Model Summary

For a 10-person group, 2M VND/period, 10 periods:

| Item | Goes to |
|---|---|
| Pot (~20M VND) | Auction winner |
| Auction spread | Split equally among non-winners |
| 1% platform fee (200K) | crosca |
| Collateral yield (50%) | Each member proportionally |
| Collateral yield (50%) | crosca |
| Unused reserve fund | Shared equally at end of season |

A member who plays honestly pays ~1% effective fee and earns back collateral yield
plus reserve bonus — net cost close to zero.

---

## Target Users

**Primary**: Urban Vietnamese aged 25–40 who have experienced or witnessed hụi fraud.
Smartphone-native, financially active, open to new tools if the safety benefit is clear.

**Secondary**: Overseas Vietnamese communities (US, Australia, Japan) who run hụi
across borders — where stablecoin payments and smart contract escrow solve the
cross-border trust and settlement problem directly.

**Not targeted initially**: Traditional hụi players who already have a trusted organiser
and see no reason to change. Conversion is possible later — not the first priority.

---

## Go-to-Market

**Phase 1 — Known groups (Q1–Q2)**
Onboard existing hụi groups via Facebook communities and offline organiser networks.
Target: 50 groups complete at least one full season.

**Phase 2 — Network expansion (Q3–Q4)**
Referral program at the group level. Organisers earn fee rebates for bringing new groups.
Target: 500 active groups, 5,000+ members with on-chain reputation.

**Phase 3 — Stranger matching (Year 2)**
Reputation-based matching engine pairs strangers into groups by trust tier.
Expand to overseas Vietnamese communities and similar ROSCA markets
(Philippines, Indonesia, West Africa).

---

## Smart Contract Architecture

---

## Competitive Landscape

| | Traditional hụi | Web2 hụi apps | ROSCA (global) | crosca |
|---|---|---|---|---|
| Who holds money | Organiser | Organiser | Organiser | Smart contract |
| Default consequences | None | None | Minimal | Automatic, permanent |
| Transparent | No | Partial | No | Fully on-chain |
| Portable credit history | No | No | No | Yes |
| Social trust preserved | Yes | Weak | Yes | Yes — by design |
| Needs bank account | No | Yes | Partial | No |

---

## Status

- [x] Protocol design complete
- [x] Smart contract architecture defined
- [ ] HuiVault.move — core escrow logic (in progress)
- [ ] CollateralManager.move
- [ ] ReputationRegistry.move
- [ ] Frontend (React Native)
- [ ] Testnet deployment on Initia

---

## Team

> [Điền tên và vai trò của team vào đây]

---

## License

MIT

