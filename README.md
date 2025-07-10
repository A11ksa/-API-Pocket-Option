<div align="center">
  <h1>API Pocket Option v2 – Python Async WebSocket Client</h1>
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

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Installation](#installation)
* [Configuration](#configuration)
* [Quick Start](#quick-start)
* [Usage Examples](#usage-examples)
* [Advanced Strategies](#advanced-strategies)
* [Session & SSID Management](#session--ssid-management)
* [Logging & Monitoring](#logging--monitoring)
* [Testing](#testing)
* [Troubleshooting](#troubleshooting)
* [Contributing](#contributing)
* [License](#license)
* [Contact](#contact)
* [Disclaimer](#disclaimer)

---

## 📖 Overview

**Pocket Option API** is a professional, fully asynchronous WebSocket API client for the Pocket Option broker. It is built for high-frequency, robust, and scalable trading bots, research tools, and automated strategies.

Key capabilities:

* Live & demo trading
* Real-time candles and tickers
* Listing available assets and payout rates
* Full trade lifecycle management (place, monitor, result)
* Automated login and session handling (including CAPTCHA)

---

## 🚀 Features

* **Asynchronous Python:** Utilizes modern `asyncio` for high performance and low latency.
* **Automatic CAPTCHA Handling:** Selenium-based login with integrated Recaptcha solver.
* **Robust WebSocket Client:** Automatic reconnect, keep-alive pings, and multi-region fallback.
* **Real-time Market Data:** Subscribe to candles, ticks, and asset updates.
* **Type-safe Models:** Built with Pydantic for data validation and auto-completion.
* **Comprehensive Logging:** Configurable logging via Loguru, with daily rotating logs.
* **Session Management:** Secure storage of SSID and credentials for demo and live accounts.
* **Flexible Configuration:** Environment variables or config files for timeouts, regions, and more.

---

## 🛠️ Installation

```bash
# Clone the repository
git clone https://github.com/A11ksa/-API-Pocket-Option.git
cd -API-Pocket-Option

# Create virtual environment
python3 -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate

# Install package
pip install .
```

Ensure you have Google Chrome installed for auto-login and CAPTCHA solving.

---

## ⚙️ Configuration

1. **Credentials & SSID**: On first run, the library will open Chrome, perform login, solve CAPTCHA, and extract SSID.

   * Credentials are stored in `sessions/config.json`.
   * Session data (SSID) is saved in `sessions/session.json`.

2. **Environment Variables**: You may override defaults via a `.env` file or environment variables:

   * `POCKET_EMAIL`, `POCKET_PASSWORD`
   * `PING_INTERVAL`, `DEFAULT_TIMEOUT`, `LOG_LEVEL`, etc.

3. **Manual SSID**: To skip auto-login, manually extract your SSID from browser DevTools and place it in `sessions/session.json`:

```json
{
  "live": "YOUR_SESSION_ID",
  "demo": "YOUR_DEMO_SESSION_ID"
}
```

---

## ⚡ Quick Start

```python
import asyncio
from api_pocket import AsyncPocketOptionClient, OrderDirection, get_ssid

async def main():
    # Retrieve SSID (auto-login)
    ssid = get_ssid(email="you@example.com", password="YourPassword")
    client = AsyncPocketOptionClient(ssid=ssid["demo"], is_demo=True)
    await client.connect()

    balance = await client.get_balance()
    print(f"Balance: {balance.balance} {balance.currency}")

    assets = await client.get_available_assets()
    for symbol, info in assets.items():
        print(symbol, "→ payout:", info.payout)

    # Place a 1-minute CALL order
    order = await client.place_order(
        asset="EURUSD_otc",
        amount=10.0,
        direction=OrderDirection.CALL,
        duration=60
    )
    print("Order placed, ID:", order.order_id)

    # Wait for result (no arbitrary timeout)
    result = await client.check_win(order.order_id)
    print("Trade finished:", result)

    await client.disconnect()

if __name__ == "__main__":
    asyncio.run(main())
```

---

## 🧑‍💻 Usage Examples

See `test4.py` for a full demonstration of all basic features. This script walks through login, balance retrieval, asset listing, order placement, and result fetching.

---

## 🤖 Advanced Strategies

Integrate with custom signal sources (e.g., Telegram, WebSocket feeds) to build automated bots:

```python
async def run_signal_bot(signal_queue):
    ssid = get_ssid(email="...")["demo"]
    client = AsyncPocketOptionClient(ssid=ssid, is_demo=True)
    await client.connect()
    while True:
        signal = await signal_queue.get()
        order = await client.place_order(
            asset=signal.asset,
            amount=signal.amount,
            direction=OrderDirection.CALL if signal.side == "CALL" else OrderDirection.PUT,
            duration=signal.duration
        )
        print(f"Executed: {order.asset} {order.direction}")
        result = await client.check_win(order.order_id)
        print("Result:", result)
```

---

## 🔒 Session & SSID Management

* **First Run:** Auto-login to generate session.
* **Subsequent Runs:** Uses saved session until expired.
* **Switching Accounts:** Update `sessions/config.json` with new credentials.

---

## 📈 Logging & Monitoring

* Logs are stored at `log-YYYY-MM-DD.txt`.
* Control verbosity with `LOG_LEVEL` (e.g., `DEBUG`, `INFO`, `ERROR`).
* Enable advanced health checks in `api_pocket/monitoring.py`.

---

## 🧪 Testing

Run the example script:

```bash
python test4.py
```

Ensure all operations complete without errors.

---

## ❓ Troubleshooting

* **Chrome fails to launch:** Verify Chrome installation and `webdriver-manager` compatibility.
* **Captcha solver hangs:** Solve manually or increase timeout.
* **WebSocket errors:** Check your network; try changing region in config.
* **Order not placed:** Ensure asset is open and sufficient balance.

For further help, open an issue on GitHub.

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository.
2. Create a feature branch.
3. Write tests and ensure formatting.
4. Submit a Pull Request.

Follow existing code style and PEP8 guidelines.

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

## 📬 Contact

* **Author:** Ahmed (<a href="mailto:ar123ksa@gmail.com">[ar123ksa@gmail.com](mailto:ar123ksa@gmail.com)</a>)
* **Telegram:** [@A11ksa](https://t.me/A11ksa)

---

## ⚠️ Disclaimer

This library is for educational and research purposes. Not affiliated with Pocket Option. Trading involves risk — use at your own discretion.
