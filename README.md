
# Crosca

**A reputation-weighted ROSCA primitive enabling reliable cooperative liquidity coordination — built on Initia.**

---

## Initia Hackathon Submission

- **Project Name**: Crosca

### Project Overview

Rotating Savings and Credit Associations (ROSCAs) are among the most widely adopted financial coordination mechanisms globally, enabling participants to accumulate savings, access lump-sum liquidity, and manage cash flow without relying on formal credit infrastructure. Despite their scale and resilience, ROSCAs remain constrained by informal trust assumptions, limited transparency, and inefficient risk allocation.

Crosca introduces a programmable ROSCA primitive that preserves the cooperative structure of traditional capital rotation while providing deterministic enforcement through smart contracts. By combining reputation-weighted collateral, auction-based liquidity allocation, and automated default resolution, Crosca ensures predictable capital outcomes for participants without relying on centralized intermediaries.

Rather than replacing the social dynamics that enable cooperative finance, Crosca formalizes them into verifiable on-chain guarantees. The result is a coordination mechanism that maintains flexibility while significantly improving reliability and scalability.

Crosca positions ROSCA as a composable DeFi primitive for structured, group-based liquidity coordination.

### Implementation Detail

- **The Custom Implementation**: Crosca implements a ROSCA state machine with reputation-weighted collateral, auction-based payout ordering, progressive collateral release, and deterministic default/archive resolution on EVM contracts. Core pool logic is implemented in `chainora-contracts/src/pool/ChainoraRoscaPool.sol` with modular runtime components for membership, contribution, auction, settlement, and archive handling.
- **The Native Feature**: Crosca integrates `initia-usernames` in the dApp profile flow (`chainora-dapp/src/pages/profile.tsx`) for username registration and identity display. For wallet and transaction UX, the app uses InterwovenKit-based connection and confirmation flow: transactions are submitted through relay-aware executors, then tracked via `waitForTxConfirmation`, and finalized with on-chain receipt checks (`waitForTransactionReceipt`) before UI state is updated.

### How to Run Locally

Prerequisites:
- Node.js `20+`, Yarn `1.22.x`, Go `1.25.x`
- Local PostgreSQL (or Supabase) for `chainora-api`
- JavaCard + Chainora native app for full end-to-end signing flow

1. Clone and init submodules:
   - `git clone https://github.com/Chainora/Crosca.git`
   - `cd Crosca`
   - `git submodule update --init --recursive`
2. Start backend (`chainora-api`):
   - `cd chainora-api`
   - `cp src/migration/config/.env.example src/migration/config/.env`
   - fill required envs in `src/migration/config/.env` (`DATABASE_URL`, `JWT_SECRET`, `CHAINORA_RPC_URL`)
   - `make tidy`
   - `make migrate`
   - `make rest`
   - optional (separate terminal): `make worker`
3. Configure and run dApp (`chainora-dapp`):
   - `cd ../../chainora-dapp`
   - `cp .env.example .env.local`
   - set required values in `.env.local` (`VITE_CHAINORA_API_URL`, `VITE_CHAINORA_RPC_URL`, `VITE_CHAINORA_CHAIN_ID`, `VITE_CHAINORA_REGISTRY_ADDRESS`, `VITE_CHAINORA_FACTORY_ADDRESS`, `VITE_CHAINORA_POOL_IMPLEMENTATION_ADDRESS`, `VITE_INTERWOVEN_NETWORK`, `VITE_INTERWOVEN_DEFAULT_CHAIN_ID`)
   - `corepack yarn install`
   - `corepack yarn dev`
Note:
- Full end-to-end signing and card-authenticated transaction flow requires a physical JavaCard and the paired native app environment.
- In judge environments without JavaCard hardware, use the provided demo video to evaluate the complete final UX and transaction lifecycle.
<img width="1587" height="786" alt="image" src="https://github.com/user-attachments/assets/06557419-08b7-4781-bcc9-b429117f3692" />

---

## Problem

| Challenge                    | Limitation in existing ROSCA systems                                                                                         |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Informal enforcement         | Participants rely on social pressure to ensure repayment, which becomes ineffective as group size or capital value increases |
| Centralised custody risk     | Funds are typically held by an organiser, introducing counterparty risk and limiting transparency                            |
| Non-portable credibility     | Historical repayment behaviour does not translate into verifiable financial reputation across groups                         |
| Uniform risk requirements    | Participants with different risk exposure often provide equivalent guarantees, resulting in inefficient capital allocation   |
| Limited transparency         | Participants cannot independently verify contribution flows or payout status                                                 |
| Weak digital implementations | Existing web2 solutions improve coordination but do not change underlying trust assumptions                                  |
| Limited scalability          | ROSCAs typically remain confined to closed social circles due to risk management constraints                                 |

---

## Solution

Crosca encodes ROSCA coordination logic into smart contracts, enabling deterministic execution of contribution flows, payout allocation, and risk resolution.

| Mechanism                          | Design Principle                                                                                                       |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Smart contract escrow              | Contributions are transferred directly into on-chain contracts, eliminating organiser custody risk                     |
| Reputation-weighted collateral     | Collateral requirements dynamically reflect each participant’s remaining payment obligation and historical reliability |
| Auction-based liquidity allocation | Participants bid a discount to access pooled capital earlier, enabling endogenous pricing of liquidity preference      |
| Social vouching                    | Existing participants can underwrite new entrants, aligning incentives through shared financial exposure               |
| Automated default resolution       | Collateral, vouch stake, and reserve funds ensure payout completion even in the presence of participant default        |
| Progressive collateral release     | Collateral requirements decrease as repayment obligations are fulfilled, improving capital efficiency                  |
| On-chain credibility accumulation  | Contribution history generates a persistent reputation signal usable across future coordination cycles                 |
| Shared reserve mechanism           | Collective buffer mitigates tail-risk scenarios without requiring excessive overcollateralization                      |

---

## Protocol Mechanism

Crosca separates capital custody from social coordination.

Participants maintain the flexibility to form groups based on existing relationships while relying on deterministic contract execution to enforce financial outcomes.

A typical coordination cycle proceeds as follows:

1. A ROSCA pool is instantiated with defined contribution size and duration
2. Participants join through vouch-backed admission or group consensus
3. Collateral requirements are calculated relative to payout position risk
4. Recurring contributions are executed directly through smart contract escrow
5. Participants bid for early access to pooled capital through a discount mechanism
6. Auction outcomes determine payout order dynamically
7. Collateral obligations decrease as repayment commitments are fulfilled
8. Default scenarios are resolved automatically through deterministic on-chain rules
9. Successful participation contributes to persistent reputation accumulation

This structure enables dynamic liquidity access while ensuring predictable capital availability for all participants.

---

## Why Initia

Crosca benefits from Initia’s architecture, which supports predictable and efficient recurring financial coordination.

Key characteristics include:

* low-latency execution suitable for periodic contribution flows
* modular design enabling specialised coordination logic
* interoperability with identity and liquidity layers
* scalable execution environment for repeated contract interaction

Group-based financial coordination requires consistent execution guarantees, making Initia an appropriate environment for ROSCA primitives.
<img width="1328" height="555" alt="image" src="https://github.com/user-attachments/assets/c3412eb5-5ca5-43b7-8bf8-b2b8f86c762a" />

---

## Smart Contract Architecture

- `ChainoraProtocolRegistry`: protocol control plane for canonical contract and adapter addresses.
- `ChainoraRoscaFactory`: deploys pool instances (clone pattern) and anchors pool creation lifecycle.
- `ChainoraRoscaPool` + modules: executable ROSCA runtime split across membership, contribution, auction, settlement, runtime sync, and default/archive modules.
- `ChainoraProtocolTimelock`: governance and delayed admin controls for critical protocol configuration updates.

Transaction lifecycle: users interact through the dApp, the factory and pool contracts enforce period transitions and payout/default rules on-chain, and frontend state is updated only after transaction confirmation and receipt validation.

## Competitive Landscape

| Feature                  | Traditional ROSCA    | Web2 ROSCA platforms | Crosca                     |
| ------------------------ | -------------------- | -------------------- | -------------------------- |
| fund custody             | organiser controlled | platform controlled  | smart contract escrow      |
| execution guarantees     | informal             | partial              | deterministic              |
| transparency             | limited              | restricted           | fully verifiable           |
| credibility portability  | absent               | absent               | persistent on-chain record |
| risk allocation          | uniform              | uniform              | dynamically weighted       |
| liquidity timing         | negotiated manually  | platform assisted    | auction-priced             |
| participant coordination | social               | platform mediated    | hybrid social-onchain      |
| scalability              | localised            | limited              | composable                 |
