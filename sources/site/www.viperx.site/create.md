# Source: https://www.viperx.site/create

[Skip to content](https://www.viperx.site/create#main)

Deploy

# Register an agent

Pick a strategy engine, name your agent, and sign one transaction. Nothing is custodied by this app.

1\. Select Network

Base SepoliaSolana Devnet

Execution mode

Live Base Sepolia registration

Register on-chain with your wallet. The app never receives custody or withdrawal authority.

LivePaper

Connect a Base Sepolia wallet to register on the EVM registry contract.

Connect

2\. Select Strategy Engine

Trend FollowingSelected

### Momentum Trend Follower

Detects directional price momentum over a rolling window (20 ticks) and opens long/short positions on threshold breakouts.

Window:20 ticks

Threshold:0.50% (50 bps)

Trade Size:$20 USD

Range Trading

### RSI Mean Reversion

Calculates Relative Strength Index (RSI). Buys oversold dips (RSI <= 35) and shorts overbought spikes (RSI >= 65), exiting at the mean.

RSI Window:14 ticks

Bounds:RSI 35 / 65

Trade Size:$25 USD

Grid Market Maker

### Automated Grid Trading

Establishes a dynamic grid around baseline market prices, placing automated buy-low and sell-high orders on grid spacing boundaries.

Grid Spacing:0.30% (30 bps)

Grid Mode:Symmetric

Trade Size:$30 USD

3\. Configure Agent Parameters

Agent ID

Unique per your wallet, immutable, up to 32 bytes.

Name

Display name, up to 64 bytes.

Strategy URI

Off-chain strategy metadata link, up to 200 bytes.

Vault Address (optional)

Owner address that holds collateral. Leave blank to default to your connected EVM wallet.

Register Agent on Base Sepolia