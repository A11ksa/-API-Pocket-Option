Setup Instructions
This document outlines the steps to set up the api-pocket-option project for interacting with the Pocket Option API.
Prerequisites

Python 3.8 or higher
pip (Python package manager)
Virtual environment (recommended)
Pocket Option SSID (extracted from browser)

Installation Steps

Clone the Repository:
git clone https://github.com/A11ksa/-API-Pocket-Option.git
cd -API-Pocket-Option


Create a Virtual Environment:
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate


Install the Package:
pip install .


Configure Environment:

Create a .env file in the root directory.
Add the SSID and API configuration:SSID=42["auth",{"session":"your_session_id","isDemo":1,"uid":"your_uid","platform":1}]
API_URL=https://api.pocketoption.com
WS_URL=wss://api.pocketoption.com/ws


To obtain the SSID, extract it from the browser as described in the Pocket Option API documentation (e.g.,).


Run Tests:
python test4.py



Notes

Ensure the sessions/ directory exists and is writable for session management.
Refer to README.md for usage examples and further details.
The SSID format is critical for authentication due to reCAPTCHA validation ().
