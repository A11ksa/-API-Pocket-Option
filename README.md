API Pocket Option

A Python library for interacting with the Pocket Option API, supporting REST and WebSocket connections for trading.

Features





REST API client for managing trades and account balance



WebSocket client with keep-alive functionality



Session persistence using JSON



SSID-based authentication (required due to reCAPTCHA)

Installation





Clone the repository:

git clone https://github.com/A11ksa/-API-Pocket-Option.git
cd -API-Pocket-Option



Follow the instructions in setup.md to set up the environment.



Install the package:

pip install .

Usage

REST API Example

from api_pocket.client import PocketClient

# Initialize client with SSID
client = PocketClient()

# Connect to API
client.connect()

# Check balance
print("Balance:", client.get_balance())

# Place a trade
trade_info = client.buy("EURUSD", amount=1, direction="call", duration=60)
print("Trade Info:", trade_info)

WebSocket Example

from api_pocket.websocket_client import WebSocketClient

# Initialize WebSocket client
ws_client = WebSocketClient()

# Connect to WebSocket
ws_client.connect()

# Send a ping message
ws_client.send_message({"type": "ping"})

Configuration





Store configuration in .env or sessions/config.json.



Example .env:

SSID=42["auth",{"session":"your_session_id","isDemo":1,"uid":"your_uid","platform":1}]
API_URL=https://api.pocketoption.com
WS_URL=wss://api.pocketoption.com/ws



Extract the SSID from the browser as shown in the setup instructions.

Directory Structure





api_pocket/: Core library files





client.py: REST API client



websocket_client.py: WebSocket client



config.py: Configuration management



utils.py: Utility functions



connection_keep_alive.py: WebSocket keep-alive



login.py: Authentication handling



exceptions.py: Custom exceptions



models.py: Data models



monitoring.py: Connection monitoring



constants.py: Constants



sessions/: Session and configuration storage





config.json: API configuration



session.json: Session data



test4.py: Test script



setup.py: Package installation script



requirements.txt: Dependencies



setup.md: Setup instructions

Contributing





Fork the repository.



Create a feature branch (git checkout -b feature-name).



Commit changes (git commit -m "Add feature").



Push to the branch (git push origin feature-name).



Open a pull request.

License

MIT License
