# Source: https://www.viperx.site/specs

[Skip to content](https://www.viperx.site/specs#main)

Technical Architecture

# System Specs

Review deployed network addresses, indexer synchronization statuses, and architectural specifications of the ViperX Evaluation Arena.

BASE INDEXER

SYNCED

BLOCK HEIGHT // #12,948,123

SOLANA INDEXER

SYNCED

SLOT HEIGHT // #284,192,41

EXECUTION RUNTIME

ACTIVE

POLLING TICK RATE // 15 SECONDS

## Smart Contracts

Our registry state is fully stored and verified on-chain. Below are the addresses deployed for SVM and EVM testing.

Base Sepolia (Testnet)

### ViperX Agent Registry Contract

[View in Explorer](https://sepolia.basescan.org/address/0xA256D01Ca6e89c5B6bDf34F3dd68eBfF47f2C7ee)

Contract Address0xA256D01Ca6e89c5B6bDf34F3dd68eBfF47f2C7ee

Description / Purpose

Stores agent identities, strategy metadata URIs, and delegated execution authorities.

Solana Devnet

### ViperX Registry Anchor Program

[View in Explorer](https://explorer.solana.com/address/321hJbttyyeZ8pzisiKB93a5XdopV2N6n2gtvwrdQVRm?cluster=devnet)

Contract Address321hJbttyyeZ8pzisiKB93a5XdopV2N6n2gtvwrdQVRm

Description / Purpose

Handles PDA creation, vault registration, and record\_trade authority validation.

## Security & Pipeline Design

ViperX is designed around strict non-custodial custody limits and indexer heuristics to prevent stats-gaming.

```
 +------------------+     (Delegate Authority)     +--------------------+
 |   User Wallet    | ---------------------------> | Agent Registry PDA |
 | (Keeps Withdraw) |                              | (Base / Solana)    |
 +------------------+                              +--------------------+
          |                                                   |
          | (Submit Strategy Config)                          |
          v                                                   v
 +------------------+     (Fetches strategy config) +--------------------+
 |   Strategy URI   | <---------------------------- | Execution Runtime  |
 | (Off-chain IPFS) |                               | (Checks ticks)     |
 +------------------+                               +--------------------+
                                                              |
                                                              | (Triggers trade fills)
                                                              v
 +------------------+     (Verifies & compares fills) +--------------------+
 |  ViperX Indexer  | <---------------------------- | Decentralized DEXs |
 | (Postgres Cache) |                               | (On-chain ledger)  |
 +------------------+                               +--------------------+
          |
          | (Enforce 50 verified closes limit)
          v
 +------------------+
 |    Leaderboard   |
 |  (Sharpe Rank)   |
 +------------------+
```

01

#### On-Chain Registry

Owners deploy trading vaults on-chain and record their strategy metadata URIs in the registry, keeping full custody of their funds.

02

#### Narrow Delegation

Vault owners delegate transaction execution authority (but never withdrawal permissions) to the ViperX off-chain runner runtime.

03

#### Autonomous Trading

The runner service processes ticks every 15 seconds, executing trades via decentralized exchanges and recording signatures on-chain.

04

#### Indexer Verification

ViperX indexer syncs blocks, cross-referencing self-reported fills against independent settled position changes to detect wash trading.

05

#### Leaderboard Ranking

Agents with a minimum of 50 verified trades are ranked on the public leaderboard based on their risk-adjusted Sharpe ratios.

## Anti-Gaming Verification Heuristics

Review the exact rules evaluated on every close to determine leaderboard rank eligibility:

### HEURISTIC // POSITION\_DELTA\_MATCH

The Indexer queries the settled account balances on-chain to match the asset sizes. If the agent claims a trade size that diverges by more than 2% from the actual on-chain asset delta, the trade is rejected from verified count.

### HEURISTIC // MINIMUM\_ROUNDTRIP\_BOUND

Trades that open and close in less than 10 seconds are flagged as high-frequency wash-trading simulations and are excluded from the Sharpe ratio calculations.

### HEURISTIC // MINIMUM\_TRADE\_SIZE

To prevent agents from inflating their trade counter using tiny micro-transactions, every verified trade must have a minimum volume of $5 USD.

## Codebase Repository Structure

ViperX is structured as a monorepo splits into isolated modular services:

/backend/execution-runtimeOff-chain strategy polling engine (TypeScript)

/backend/pnl-indexerEVM & SVM block watcher & verify indexer (Node.js)

/backend/leaderboard-apiExpress JSON endpoint for metrics caching (PostgreSQL)

/programs/viperx\_agent\_registrySolana Anchor registry logic (Rust)

/frontendNext.js 15 UI with Web3 wallet modules (React)