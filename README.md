# Moodle-Notifier
Automatic Windows desktop notifications for Moodle assignment deadlines alerts at 12h, 6h, 1h, 30min and 10min before due date, with submission detection via the Moodle REST API.

## Features

- 🟡 12h and 6h warnings
- 🟠 1 hour warning
- 🔴 30 minute warning
- ⚠️ 10 minute warning
- ✅ Submission detected — notified automatically when you submit a task
- State persistence — tracks which notifications have already been sent, no duplicates
- Activity log — full log file for debugging and history
- Fully configurable — thresholds and check interval editable in one file

---

## Requirements

- Windows 10 / 11
- Python 3.8 or higher
- A Moodle instance with the REST API enabled (`moodle_mobile_app` web service)

---

## Installation

1.Install dependencies

Double-click "install.bat" or run manually:

2.Configure your credentials

Open "config.py" and fill in your details:

MOODLE_URL      = "https://your-moodle-instance.com/moodle" 
MOODLE_USERNAME = "your_username"
MOODLE_PASSWORD = "your_password"
```

4. Run

Double-click "run.bat", or:


⚙️ Configuration

All settings are in `config.py`:

| Setting | Default | Description |
|---|---|---|
| "MOODLE_URL" | — | Base URL of your Moodle instance |
| "MOODLE_USERNAME" | — | Your Moodle username |
| "MOODLE_PASSWORD" | — | Your Moodle password |
| "NOTIFICATION_THRESHOLDS" | "720, 360, 60, 30, 10]" | Minutes before deadline to notify |
| "CHECK_INTERVAL_MINUTES" | "5" | How often to poll Moodle (in minutes) |
| "STATE_FILE" | "notifier_state.json" | File used to persist notification state |

---

##  Auto-start with Windows

To have Moodle Notifier launch automatically when you log in:

1. Press "Win + R", type "shell:startup" and press Enter
2. Create a shortcut to "run.bat" inside that folder

---

## Project Structure

```
moodle-notifier/
│
├── main.py               # Main loop and notification logic
├── moodle_api.py         # Moodle REST API client
├── notifier.py           # Windows desktop notification handler
├── tracker.py            # Notification state persistence
├── config.py             # ⚙️  All user configuration (edit this)
│
├── install.bat           # Dependency installer for Windows
├── run.bat               # Launch script for Windows
│
├── notifier_state.json   # Auto-generated — do not delete
└── moodle_notifier.log   # Auto-generated activity log
```

---

##  How It Works

```
Every CHECK_INTERVAL_MINUTES:
  │
  ├── Authenticate with Moodle REST API
  ├── Fetch enrolled courses
  ├── Fetch all assignments with a due date
  │
  └── For each assignment:
        ├── If submitted and not yet notified → notify submission
        └── If time left ≤ threshold and not yet notified → notify deadline
```

State is persisted in "notifier_state.json" so notifications are never sent twice, even after restarting the script.

---

## 🛠️ Troubleshooting

Notifications don't appear
Go to Windows Settings → System → Notifications and make sure Python is allowed to send notifications.

Login error
Double-check your username and password in `config.py`. Also verify your Moodle has the `moodle_mobile_app` web service enabled (ask your admin).

Check the log
Open `moodle_notifier.log` to see exactly what the script is doing on each cycle.

---

## License

MIT License — free to use, modify and distribute.

https://github.com/angelamat178?tab=repositories
