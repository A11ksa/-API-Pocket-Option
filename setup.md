
# 🛠️ Setup & Installation Guide

This document outlines the steps to set up the **Pocket Option API** project for professional interaction with Pocket Option's WebSocket API.

---

## 📋 Prerequisites

- **Python 3.8 or higher** (recommended 3.9+)
- `pip` (Python package manager)
- [Google Chrome](https://www.google.com/chrome/) (for automatic SSID extraction)
- (Optional) **virtualenv** for isolated environments

---

## 🚀 Installation Steps

1. **Clone the Repository:**

    ```bash
    git clone https://github.com/A11ksa/-API-Pocket-Option.git
    cd -API-Pocket-Option
    ```

2. **Create & Activate a Virtual Environment:**

    ```bash
    python -m venv venv
    # On Linux/macOS:
    source venv/bin/activate
    # On Windows:
    venv\\Scripts\\activate
    ```

3. **Install the Package and Dependencies:**

    ```bash
    pip install .
    ```

---

## ⚙️ **Configuration**

1. **SSID / Session Setup (Recommended)**

    - The first run will ask for your Pocket Option credentials.
    - The library will launch Chrome, solve reCAPTCHA automatically, and save the SSID/session to `sessions/session.json`.

2. **Manual SSID Setup (Alternative):**
    - You can manually copy your SSID/session string from your browser (after logging into Pocket Option) and put it in `sessions/session.json`.

---

## 🔧 **Environment Variables (Optional)**

You can set parameters via environment variables or in a `.env` file in the project root:

| Variable             | Description                            | Default             |
|----------------------|----------------------------------------|---------------------|
| `PING_INTERVAL`      | Ping interval (seconds)                | `20`                |
| `MIN_ORDER_AMOUNT`   | Minimum order amount                   | `1.0`               |
| `MAX_ORDER_AMOUNT`   | Maximum order amount                   | `50000.0`           |
| `DEFAULT_TIMEOUT`    | API default timeout (seconds)          | `30.0`              |
| `LOG_LEVEL`          | Logging level                          | `INFO`              |

---

## 🧪 **Testing the Installation**

Run the included test script to ensure everything works:

```bash
python test4.py
