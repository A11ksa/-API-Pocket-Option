README = """
<div align="center">
  <img src="https://img.icons8.com/color/96/pocket-option.png" width="80" />
  <h1>Pocket Option API (v2) – Python Async WebSocket Client</h1>
  <p>
    <b>⚡️ Professional, fully-async trading API for Pocket Option broker ⚡️</b><br>
    <img src="https://img.shields.io/pypi/pyversions/pandas?label=python&logo=python" />
    <img src="https://img.shields.io/github/license/A11ksa/-API-Pocket-Option?style=flat-square" />
    <img src="https://img.shields.io/badge/async-supported-brightgreen?logo=python"/>
    <img src="https://img.shields.io/badge/recaptcha-auto-blue"/>
    <img src="https://img.shields.io/badge/status-stable-success?logo=github"/>
  </p>
</div>

---

## 🚀 Features

- **Fully Asynchronous WebSocket Client** – lightning-fast trade execution.
- **Advanced Error Handling** and auto-reconnect with keep-alive.
- **Smart Session Management**: auto-login with CAPTCHA solving.
- **Type-Safe Models**: Data validation via Pydantic.
- **Live & Demo**: Switch between live and demo accounts easily.
- **Asset Scanner**: Real-time payout extraction for all assets.
- **Detailed Logging & Health Monitoring**.
- **Modern Python Practices** (type hints, dataclasses, black-formatting).

---

## 📦 Installation

```bash
git clone https://github.com/A11ksa/-API-Pocket-Option.git
cd -API-Pocket-Option
python -m venv venv
source venv/bin/activate  # (or venv\\Scripts\\activate on Windows)
pip install .


| Path                                  | Description                         |
| ------------------------------------- | ----------------------------------- |
| `api_pocket/client.py`                | Main async API client               |
| `api_pocket/login.py`                 | SSID/session/captcha utilities      |
| `api_pocket/models.py`                | Type-safe models (Order, Candle...) |
| `api_pocket/connection_keep_alive.py` | Persistent connection manager       |
| `api_pocket/monitoring.py`            | Health/error monitoring             |
| `api_pocket/websocket_client.py`      | Async WebSocket core                |
| `sessions/`                           | Session/config files                |
| `test4.py`                            | Example usage / test script         |
| `requirements.txt`                    | Python dependencies                 |
| `setup.py`                            | Installation script                 |


import asyncio
from api_pocket import AsyncPocketOptionClient, OrderDirection, get_ssid

async def main():
    # SSID retrieval (auto-handles CAPTCHA via Selenium)
    ssid = get_ssid(email="your@email.com", password="your_password")["demo"]
    
    # Init client (demo=True for demo, False for live)
    client = AsyncPocketOptionClient(ssid=ssid, is_demo=True)
    await client.connect()
    
    # Get balance
    balance = await client.get_balance()
    print(f"Balance: {balance.balance} {balance.currency}")

    # Get all assets and payouts
    assets = await client.get_available_assets()
    print("Assets and payouts:", assets)

    # Place an order (for example, 10$ CALL on EURUSD_otc for 1min)
    order_result = await client.place_order(
        asset="EURUSD_otc",
        amount=10,
        direction=OrderDirection.CALL,
        duration=60
    )
    print("Order placed:", order_result)

    # Wait for the trade result (Win/Loss/Draw)
    win_result = await client.check_win(order_result.order_id)
    print("Order finished:", win_result)

    await client.disconnect()

if __name__ == "__main__":
    asyncio.run(main())


| Setting           | File/Env              | Description                       |
| ----------------- | --------------------- | --------------------------------- |
| SSID/session      | sessions/session.json | User session for authentication   |
| Credentials       | sessions/config.json  | Email/password storage (optional) |
| Config parameters | .env or environment   | (Ping, Timeout, Amount, etc)      |

