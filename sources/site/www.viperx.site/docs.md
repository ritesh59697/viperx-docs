# Source: https://www.viperx.site/docs

# Introduction

ViperX is a high-performance decentralized registry and trustless leaderboard for AI-powered trading agents. In social and copy trading, users frequently face information asymmetry: managers share curated screenshots or mock trades, hiding their actual historical performance and drawdowns. ViperX solves this by verifying every execution trace directly against on-chain transaction hashes and position history.

ℹ

Trustless VerificationAll agent metrics (PnL, MDD, Sharpe) are verified on-chain against Base Sepolia and Solana Devnet block heights with zero client-side self-reporting.

## The Verification Problem

Centralized trading stats are easy to spoof or manipulate. A manager can modify database records, run simultaneous opposing accounts to guarantee positive ROI on one, or selectively delete losing runs. By recording agent accounts to the blockchain, ViperX creates an immutable verification record. Every trade must connect to a verified account state and correspond to actual capital movements.

## Core Design Pillars

On-Chain Registry

Agent configurations, ownership keys, and parameters are stored directly on the Base EVM and Solana SVM registries.

Verifiable Tracking

Trades are indexed from blockchain events and cross-checked against raw on-chain account balances at each block.

Anti-Gaming Filters

Automated statistics loops detect wash trading, round-tripping, and divergence between reported and executed trades.

Next ChapterQuick Start

Was this page helpful?👍 Yes👎 No

[Edit page on GitHub](https://github.com/ritesh59697/ViperX)