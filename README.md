<div align="center">
  <img src="https://img.icons8.com/color/96/pocket-option.png" width="80" />
  <h1>Pocket Option API v2 – Python Async WebSocket Client</h1>
  <p>
    <b>⚡ Professional, fully asynchronous trading API for Pocket Option broker ⚡</b><br>
    <img src="https://img.shields.io/pypi/pyversions/pandas?label=python&logo=python" />
    <img src="https://img.shields.io/github/license/A11ksa/-API-Pocket-Option?style=flat-square" />
    <img src="https://img.shields.io/badge/async-supported-brightgreen?logo=python"/>
    <img src="https://img.shields.io/badge/recaptcha-auto-blue"/>
    <img src="https://img.shields.io/badge/status-stable-success?logo=github"/>
  </p>
</div>

---

# Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Why Use This Library?](#why-use-this-library)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration & Setup](#configuration--setup)
- [Basic Usage Example](#basic-usage-example)
- [Advanced Usage: Custom Strategies](#advanced-usage-custom-strategies)
- [Session & SSID Management](#session--ssid-management)
- [Logging & Monitoring](#logging--monitoring)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)
- [Disclaimer](#disclaimer)

---

## 📖 Overview

**Pocket Option API** is a professional, fully asynchronous WebSocket API client for the Pocket Option broker, designed for high-frequency, robust, and scalable trading bots and research tools.

This library allows you to interact with the Pocket Option platform just like the official web client, including:
- **Live & demo trading**
- Retrieving real-time candles/tickers
- Listing available assets and payout rates
- Full trade management (open, monitor, result)
- Automated login and session handling (including CAPTCHA)

---

## 🚀 Features

- **Asynchronous Python:** Fast and scalable, using modern async/await paradigms.
- **Automatic CAPTCHA Handling:** Integrated with Selenium and Recaptcha solver.
- **Robust WebSocket Client:** Auto-reconnect, keep-alive, and multi-region fallback.
- **Real-time Market Data:** Candles, ticks, assets, payouts — all up-to-date.
- **Type-safe Models:** All data is validated with Pydantic.
- **Comprehensive Logging:** Loguru integration for detailed logs, errors, and events.
- **Session Management:** Securely stores SSID and credentials, auto-resumes sessions.
- **Flexible Config:** Works with both live and demo accounts, full environment support.
- **Plug & Play:** Simple, documented interface for both beginners and advanced users.

---

## 💡 Why Use This Library?

- **Build trading bots that react instantly to market changes (async event loop, no lag)**
- **Bypass the web GUI and interact programmatically with all Pocket Option features**
- **Fully compatible with both demo and live trading environments**
- **Automate everything: login, balance checks, order placement, monitoring, and more**
- **Run on servers, local machines, or in the cloud (no graphical interface required)**

---

## 🗂️ Project Structure

| Path                             | Description                               |
|-----------------------------------|-------------------------------------------|
| `api_pocket/client.py`            | Main async API client logic               |
| `api_pocket/login.py`             | SSID/session, login & CAPTCHA utilities   |
| `api_pocket/models.py`            | Type-safe models (Order, Candle, etc.)    |
| `api_pocket/connection_keep_alive.py` | Persistent connection manager         |
| `api_pocket/monitoring.py`        | Health/error monitoring                   |
| `api_pocket/websocket_client.py`  | Async WebSocket core                      |
| `sessions/`                       | Session/config files                      |
| `test4.py`                        | Example usage / test script               |
| `requirements.txt`                | Python dependencies                       |
| `setup.py`                        | Installation script                       |

---

## 📋 Requirements

- **Python 3.8+** (recommended 3.9+)
- Google Chrome (for auto-login/recaptcha)
- All required packages in `requirements.txt`

---

## 🛠️ Installation

```bash
git clone https://github.com/A11ksa/-API-Pocket-Option.git
cd -API-Pocket-Option
python -m venv venv
source venv/bin/activate        # (or venv\Scripts\activate on Windows)
pip install .

⚙️ Configuration & Setup
1. Auto-Login and Session Handling
On first run, the library will launch Chrome via Selenium, login to Pocket Option, solve the CAPTCHA (if any), and extract the required SSID/session automatically.
Your credentials (email & password) are stored securely in sessions/config.json, and your session in sessions/session.json.

2. Manual SSID Setup (Advanced)
Alternatively, you may extract your session manually from your browser and place it in sessions/session.json.

3. Environment Variables & Settings
You may configure parameters in a .env file or via environment variables, such as:

PING_INTERVAL, DEFAULT_TIMEOUT, LOG_LEVEL, etc.

🧑‍💻 Basic Usage Example

import asyncio
from api_pocket import AsyncPocketOptionClient, OrderDirection, get_ssid

async def main():
    # Automatically retrieve SSID (auto-login)
    ssid = get_ssid(email="your@email.com", password="your_password")["demo"]
    client = AsyncPocketOptionClient(ssid=ssid, is_demo=True)
    await client.connect()

    balance = await client.get_balance()
    print(f"Balance: {balance.balance} {balance.currency}")

    # List assets & payouts
    assets = await client.get_available_assets()
    for k, v in assets.items():
        print(k, "payout:", v['payout'], "is_open:", v['is_open'])

    # Place an order (10$ CALL on EURUSD_otc for 1min)
    order = await client.place_order(
        asset="EURUSD_otc",
        amount=10,
        direction=OrderDirection.CALL,
        duration=60
    )
    print("Order placed:", order.order_id)

    # Wait for result (no arbitrary timeout!)
    result = await client.check_win(order.order_id)
    print("Trade finished:", result)

    await client.disconnect()

if __name__ == "__main__":
    asyncio.run(main())

🤖 Advanced Usage: Custom Strategies
You can build a fully automated bot by integrating this client into your strategy logic.
Example: Follow a signal source and auto-place trades:

async def run_signal_bot(signal_queue):
    ssid = get_ssid(email="...")["demo"]
    client = AsyncPocketOptionClient(ssid=ssid, is_demo=True)
    await client.connect()
    while True:
        signal = await signal_queue.get()  # Get signal from Telegram, file, etc.
        order = await client.place_order(
            asset=signal['asset'],
            amount=signal['amount'],
            direction=OrderDirection.CALL if signal['type'] == "CALL" else OrderDirection.PUT,
            duration=signal['duration']
        )
        print(f"Trade executed: {order.asset} | {order.direction}")
        result = await client.check_win(order.order_id)
        print("Result:", result)

You can subscribe to real-time candle/tick data for AI/ML strategies as well.

🔒 Session & SSID Management
First login: Auto-extracts and saves session (with CAPTCHA handling).

Subsequent runs: Uses saved session unless expired.

Supports both demo and live accounts.

All data stored in /sessions/ for security and persistence.

📝 Logging & Monitoring
All operations and errors are logged to daily files via Loguru.

Health and error monitoring for debugging and stability.

You can control the logging level and output via environment variables.

🧪 Testing
The included test4.py script demonstrates all basic features and can be used to verify correct installation.

bash
نسخ
تحرير
python test4.py
❓ Troubleshooting
Chrome not launching / SSID extraction fails:

Make sure Google Chrome is installed and up-to-date.

Ensure chromedriver matches your Chrome version (handled by webdriver-manager).

If CAPTCHA solving fails, complete it manually in the opened browser.

WebSocket connection errors:

Check your internet/firewall settings.

If persistent, try changing WebSocket region in the config.

API errors / trade not executed:

Make sure account has enough balance.

Check if the asset is open and available for trading.

For more help, open an issue on GitHub or contact the author.

🤝 Contributing
Pull requests, bug reports, and feature suggestions are welcome!
Please open an issue or a PR, and follow the code style of the project.

📄 License
This project is licensed under the MIT License. See LICENSE for details.

📬 Contact
Author: Ahmed Althuwaini

Email: ar123ksa@gmail.com

Telegram: @A11ksa

⚠️ Disclaimer
This project is for educational and research purposes. It is not affiliated with Pocket Option or any financial broker.
Use at your own risk. Trading is risky and you are solely responsible for your actions.
