<h1 align="center">Telegram Forward Message (Telethon + asyncio)</h1>
<p align="center">
  Forward messages automatically from one source group/channel to multiple target groups with random delays, loop interval, and multi-account support.
</p>

<p align="center">
  <a href="https://www.python.org/"><img alt="Python" src="https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white"></a>
  <a href="https://github.com/LonamiWebs/Telethon"><img alt="Telethon" src="https://img.shields.io/badge/Telethon-Client-6C8EBF?logo=telegram&logoColor=white"></a>
  <img alt="AsyncIO" src="https://img.shields.io/badge/asyncio-concurrency-000000">
  <img alt="Config" src="https://img.shields.io/badge/Config-JSON-informational">
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/License-MIT-green"></a>
</p>

---

## Features
- **Multi-account** login (each phone number has its own session)
- **Auto loop** to check new messages on a configurable interval
- **Multi-target**: forward to many groups/channels at once
- **Flexible source**: use a fixed `message_id` **or** always forward the latest message
- **Human-like random delay** per forward (`min_delay``max_delay`)
- **Access validation** for source and target groups before sending
- **Simple JSON configuration** (`configuration.json`)

---

## Requirements
- Python **3.10+**
- Telegram account (2FA supported)
- `api_id` and `api_hash` from <https://my.telegram.org>  *API Development Tools*

---

## Quick Start

```bash
git clone https://github.com/Nadir-N3/Telegram-forward-message.git
cd Telegram-forward-message

# (Optional but recommended)
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

# Install dependencies
pip install telethon
```

---

## Create Sessions & Config (Mode 1)
Run the script and choose **1** to create sessions and build `configuration.json` interactively:

```bash
python main.py
# Choose: 1
```

The wizard will ask for:
- Number of accounts for each: `phone`, `api_id`, `api_hash`
- **Source group** (`-100...` id or `@username`)
- **Message mode**:
  - A fixed `message_id` (forward the same message every loop), **or**
  - Leave empty to always forward the **latest** message
- **Target groups** list (ids or `@username`)
- **Random delay** range (`min_delay`, `max_delay`)
- **Loop interval** for checking new messages

It will:
- Log you in (OTP 2FA if enabled)
- Save per-account sessions (e.g., `session_628XXXXXX.session`)
- Write **`configuration.json`**

---

## Run the Bot (Mode 2)
After `configuration.json` exists:

```bash
python main.py
# Choose: 2
```

The bot will:
- Print your current configuration
- Validate access to source/targets
- Fetch the fixed/latest message based on your mode
- Forward to all targets with a random delay per send
- Sleep and repeat according to `loop_interval`

---

## Example `configuration.json`

```json
{
  "accounts": [
    {
      "api_id": "123456",
      "api_hash": "abcdef1234567890abcdef1234567890",
      "phone": "+628123456789",
      "session": "session_628123456789"
    },
    {
      "api_id": "789012",
      "api_hash": "1234567890abcdef1234567890abcdef",
      "phone": "+628987654321",
      "session": "session_628987654321"
    },
    {
      "api_id": "345678",
      "api_hash": "abcdefabcdef1234567890abcdef1234",
      "phone": "+628555555555",
      "session": "session_628555555555"
    }
  ],
  "groups": [
    "-100123456789",
    "@TargetGroup1",
    "-100345678901",
    "@TargetGroup2"
  ],
  "source_group": "-100987654321",
  "message_id": null,
  "delay": {
    "min_delay": 60.0,
    "max_delay": 120.0
  },
  "loop_interval": 300.0
}
```

### Field Notes
- `accounts[]` list of accounts (each with its own `.session`)
- `source_group` source group/channel id or `@username`
- `message_id` number for a specific message; `null` to always use the latest
- `use_fixed_message_id` optional, defaults to `false`
- `groups[]` target groups/channels (ids or `@username`)
- `delay.min_delay` / `delay.max_delay` random delay in seconds between forwards
- `loop_interval` seconds to wait before checking for a new message (recommended 60)

---

## Troubleshooting
- **`PhoneCodeInvalidError`** OTP code is wrong/expired; request a new one.  
- **`SessionPasswordNeededError`** Your account uses 2FA; enter your 2FA password.  
- **`ChannelPrivateError` / `ChatAdminRequiredError`** The account a member or lacks permission in that group.  
- **No new message** Ensure a new message was posted (newer than the last forwarded id).  
- **Flood/Rate limits** Increase delays, reduce number of targets, or switch accounts.  
- **Windows encoding issues** Use a UTF-8 terminal (e.g., new Windows Terminal/PowerShell).

---

## Usage & Policy
- Respect **Telegram Terms of Service**.
- Avoid spam; pick reasonable delay/interval values.
- Only forward content you have permission to share.

---

## License
Licensed under **MIT**. See [LICENSE](LICENSE) for details.
