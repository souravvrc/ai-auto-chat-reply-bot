# 🤖 WhatsApp AI Chat Bot

A Python automation bot that monitors a WhatsApp Web conversation, detects incoming messages, and auto-replies using OpenAI GPT — mimicking a real person's texting style. Built with PyAutoGUI for screen automation and clipboard manipulation.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 👁️ **Chat Monitoring** | Continuously watches a WhatsApp Web chat for new messages |
| 🧠 **AI-Powered Replies** | Generates context-aware responses using OpenAI GPT-3.5 Turbo |
| 💬 **Persona-Based Responses** | Bot replies as a specific character (Naruto) — funny, roast-style |
| 📋 **Clipboard Automation** | Uses clipboard to read chat history and send replies |
| 🖱️ **Full GUI Automation** | Clicks, drags, selects, copies, pastes — all without manual input |
| 🔁 **Continuous Loop** | Polls every 5 seconds and responds only when the target sender wrote last |

---

## 🗂️ Project Structure

```
whatsapp-ai-bot/
│
├── 01_get_cursor.py    # Utility to capture mouse coordinates for calibration
├── 02_openai.py        # Standalone test — GPT persona response from chat history
├── 03_bot.py           # Main bot — screen automation + AI reply loop
└── README.md           # Project documentation
```

---

## 🔁 How It Works

```
Every 5 seconds:
        │
        ▼
  Move & drag mouse over WhatsApp chat area
        │
        ▼
  Ctrl+C → copy chat history to clipboard
        │
        ▼
  Check if last message is from target sender ("Rohan Das")
        │
       Yes
        │
        ▼
  Send chat history to OpenAI GPT with persona prompt
        │
        ▼
  Copy AI response → click message box → Ctrl+V → Enter
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- WhatsApp Web open in a browser (Chrome recommended)
- OpenAI API key

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/souravvrc/ai-auto-chat-reply-bot.git
   cd whatsapp-ai-bot
   ```

2. **Install dependencies**
   ```bash
   pip install openai pyautogui pyperclip
   ```

3. **Add your OpenAI API key**

   In `03_bot.py` and `02_openai.py`, replace:
   ```python
   api_key="<Your Key Here>"
   ```
   with your actual key (or use an environment variable — see security note below).

---

## 🖱️ Calibration (Important!)

The bot uses **hardcoded screen coordinates** to click and drag on WhatsApp Web. You must calibrate these for your screen resolution.

**Step 1 — Run the cursor tracker:**
```bash
python 01_get_cursor.py
```
This prints your mouse position in real time. Hover over key areas and note the coordinates:

| Element | What to find |
|---------|-------------|
| Chrome taskbar icon | Where to click to focus the browser |
| Top-left of chat area | Start of the drag selection |
| Bottom-right of chat area | End of the drag selection |
| Message input box | Where to click before pasting |
| Browser tab / focus area | Click to deselect after copy |

**Step 2 — Update coordinates in `03_bot.py`:**
```python
pyautogui.click(1639, 1412)        # Chrome icon
pyautogui.moveTo(972, 202)         # Chat area top-left
pyautogui.dragTo(2213, 1278, ...)  # Chat area bottom-right
pyautogui.click(1994, 281)         # Tab / focus click
pyautogui.click(1808, 1328)        # Message input box
```

---

## ▶️ Running the Bot

1. Open **WhatsApp Web** in Chrome and navigate to the target chat
2. Run:
   ```bash
   python 03_bot.py
   ```
3. The bot will auto-click Chrome, select the chat, and start monitoring

> ⚠️ Don't move your mouse while the bot is running — PyAutoGUI controls your cursor.

---

## 🧪 Testing the AI Persona

Before running the full bot, test the GPT persona in isolation:
```bash
python 02_openai.py
```
This sends a sample WhatsApp chat log to GPT and prints how "Harry" would respond — useful for tuning your system prompt.

---

## 🔑 API Key Security

> ⚠️ **Never commit your API key to GitHub.**

Use environment variables instead:
```python
import os
client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
```

Or use a `.env` file with `python-dotenv`:
```bash
pip install python-dotenv
```
```python
from dotenv import load_dotenv
load_dotenv()
client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
```
Add `.env` to your `.gitignore`:
```
.env
```

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)
![PyAutoGUI](https://img.shields.io/badge/PyAutoGUI-FF6F00?style=for-the-badge&logo=python&logoColor=white)

| Library | Role |
|---------|------|
| `pyautogui` | Controls mouse and keyboard for GUI automation |
| `pyperclip` | Reads and writes clipboard content |
| `openai` | GPT-3.5 Turbo for persona-based reply generation |
| `time` | Manages polling delays between checks |

---

## ⚠️ Disclaimer

This project is built for **educational and personal fun purposes only**. Automating WhatsApp messages may violate [WhatsApp's Terms of Service](https://www.whatsapp.com/legal/terms-of-service). Use responsibly and only in private chats with consent.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---
