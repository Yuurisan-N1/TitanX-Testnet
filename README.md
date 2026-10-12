<div align="center">

<img width="100%" alt="header" src="https://capsule-render.vercel.app/api?type=waving&height=210&text=TitanX%20Testnet%20Bot&fontAlign=50&fontAlignY=36&fontSize=56&desc=Wallet%20Sign%20In%20%7C%20Continuous%20Entry%20%7C%20Attached%20TP%20SL%20%7C%20Live%20PnL&descAlign=50&descAlignY=58"/>

<img alt="typing" src="https://readme-typing-svg.demolab.com?font=Inter&size=18&duration=3000&pause=650&center=true&vCenter=true&width=900&lines=Wallet+Signature+Sign+In+%7C+No+Browser+Session;Perpetual+Entry+At+Maximum+Leverage+%7C+Random+Side;Take+Profit+And+Stop+Loss+Resting+On+The+Engine;Continuous+Re+Entry+%7C+Live+PnL+Line;Built+by+Yuurisandesu"/>

<p>
  <img alt="python" src="https://img.shields.io/badge/Python-3.12%2B-3776AB?logo=python&logoColor=white"/>
  <img alt="platform" src="https://img.shields.io/badge/Platform-TitanX%20Testnet-111111"/>
  <img alt="wallet" src="https://img.shields.io/badge/Wallet-Single%20Key-111111"/>
  <img alt="proxy" src="https://img.shields.io/badge/Proxy-Supported-111111"/>
  <img alt="author" src="https://img.shields.io/badge/by-Yuurisandesu-111111"/>
</p>

<p>
  <b>TitanX Bot</b> is a full automation bot for the TitanX perpetual futures testnet.<br/>
  It signs in with a wallet key, opens a position at the highest leverage the market allows on a randomly picked pair, attaches the take profit and the stop loss to the entry order, and enters again the moment the position is closed, all with proxy support and a live PnL line.<br/>
  Built and distributed by <b>Yuurisandesu</b>.
</p>

</div>

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Bot](#running-the-bot)
- [Features](#features)
- [File Structure](#file-structure)
- [Disclaimer](#disclaimer)

---

## Requirements

- Python `3.12+`
- Git

---

## Installation

### Install Python

**Windows:**

1. Download the installer from https://www.python.org/downloads/ and run it
2. On the first screen tick **Add python.exe to PATH**, then continue with the default options
3. Open Command Prompt and verify the installation:

```cmd
python --version
```

**Linux (Ubuntu / Debian):**

```bash
sudo apt-get update
sudo apt-get install -y python3 python3-pip
python3 --version
```

**macOS:**

```bash
brew install python
python3 --version
```

> If you do not have Homebrew, install it first from https://brew.sh

**Termux (Android):**

```bash
pkg update && pkg upgrade -y
pkg install python -y
python --version
```

---

### Clone and Install

**Clone the repository:**

```bash
git clone https://github.com/Yuurisan-N1/TitanX-Testnet.git
cd TitanX-Testnet
```

**Install dependencies:**

```bash
pip install -r requirements.txt
```

---

## Configuration

### 1. Private Key (.env)

Rename `.env.example` to `.env` and fill in your wallet private key:

```env
PRIVATEKEY=0x
```

The bot signs in with a wallet signature and never needs a browser session or any initData, so the key must belong to the same wallet you use on the site.

### 2. Proxy (proxy.txt)

Fill `proxy.txt` with proxies, one per line (optional, leave empty to run without proxy):

```
host:port
host:port:user:pass
http://user:pass@host:port
```

The first line is used for the run, and proxy credentials are masked in the log output.

### 3. Bot Settings (config.json)

`pairs` selects the markets the bot is allowed to enter, and `min_profit_percent` and `min_loss_percent` are the close targets the take profit and the stop loss are placed from.

---

## Running the Bot

```bash
python bot.py
```

Press `Ctrl+C` at any time to stop the bot cleanly.

---

## Features

### Wallet Sign In

The bot signs in with the key from the env file and never needs a browser session or any initData. The session message is signed locally and the account is authorized on the exchange, then the balance, the leverage and the pair list are reported before the first entry. A wallet that is still unfunded claims the faucet first, while an account that is already funded skips the claim with a yellow line instead of erroring out.

### Continuous Entry

Only the pairs set to true in the config are eligible, one of them is picked at random for every entry and the side is random too, so the flow is never biased long or short. The order price is derived from the mark price and the size is the largest the balance can carry on that pair, opened at the highest leverage the market allows. There is no sleep between entries: as soon as a position closes the next one is opened with a fresh price.

### Attached Take Profit And Stop Loss

Both levels are sent inline with the entry order, so the exchange engine owns them and closes the position the moment either level is touched. That keeps the position protected even when the bot is stopped and removes any need to poll for a close. The triggers are placed from the same price the order is sent at and snapped onto the market tick grid, so the engine accepts them on the first attempt instead of refusing the order.

### Live PnL

While a position is open the bot prints a single PnL line that updates in place, holding the unrealized PnL of that open position alone. Only the number is colored, green on profit and red on loss, and the line is closed out with the realized amount once the position is gone.

### Retry And Error Reporting

An entry that does not fill is retried at a fresh price. An entry the engine refuses is retried with a wider trigger guard, and when it still fails the reason the server returned is printed on one line together with its request id, so a rejection can be traced without reading the whole log.

### Proxy Support

Proxies are loaded from `proxy.txt`, one per line, and the first line is used for the run. Credentials are never printed, the log only shows a masked form, and an empty file runs the bot direct.

---

## File Structure

```text
TitanX-Testnet/
├── bot.py          # Main bot, entry loop and position watch
├── config.json     # Eligible pairs and close targets
├── .env            # Wallet private key, PRIVATEKEY=0x
├── proxy.txt       # Proxy list, one per line (optional)
├── LICENSE         # License file
└── utils/
    └── banner.py   # Banner using yuurisan module
```

---

## Disclaimer

This tool is built for educational and technical exploration purposes. Use it wisely and at your own responsibility.

---

<div align="center">
<img width="100%" alt="footer" src="https://capsule-render.vercel.app/api?type=waving&height=120&section=footer"/>
</div>
