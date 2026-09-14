# Data Capture Workflow (n8n)

This is my first n8n project! I'm learning n8n by building small automations and documenting what I learn along the way in this repo.

## 📌 What This Workflow Does

This workflow captures a message sent to a Telegram bot, saves the sender's details into a Google Sheet, sends a confirmation email, and replies to the user on Telegram to let them know their message was received.

In short, it's a simple **lead/data capture pipeline**:

```
Telegram Message → Extract Data → Save to Google Sheet → Send Email → Reply on Telegram
```

## 🔧 How It Works (Node by Node)

1. **Telegram Trigger**
   Listens for new `message` events sent to my Telegram bot. This is what kicks off the whole workflow.

2. **Edit Fields (Set node)**
   Pulls out the useful bits from the incoming Telegram payload and renames them into clean variables:
   - `name` → sender's first name
   - `id` → sender's Telegram chat ID (needed later to reply to them)
   - `message` → the actual text they sent

3. **Append row in sheet (Google Sheets)**
   Adds a new row to a Google Sheet called **"lead capture sheet"** (tab: *normal customers*) with the captured `name`, plus an `email` and `phone number`.

4. **Send a message (Gmail)**
   Sends an email using the captured data — the subject is `"hello"` and the body contains the message the user sent on Telegram.

5. **Send a text message (Telegram)**
   Replies back to the same Telegram chat with a confirmation: *"your message has been received"*.

## 🗂️ Nodes Used

| Node | Type | Purpose |
|------|------|---------|
| Telegram Trigger | `telegramTrigger` | Starts the workflow on new message |
| Edit Fields | `set` | Cleans/maps incoming data |
| Append row in sheet | `googleSheets` | Stores data |
| Send a message | `gmail` | Sends an email |
| Send a text message | `telegram` | Sends confirmation back to user |

## ⚙️ Setup / Requirements

To run this workflow yourself, you'll need:

- An [n8n](https://n8n.io/) instance (cloud or self-hosted)
- A **Telegram Bot** (created via [@BotFather](https://t.me/BotFather)) and its API credentials
- A **Google account** with access to Google Sheets (OAuth2 credentials in n8n)
- A **Gmail account** connected via OAuth2 in n8n
- A Google Sheet set up with columns: `name`, `email`, `phone number`

> ⚠️ Note: In this version, the `email` and `phone number` fields are hardcoded as placeholders for testing. In a real setup, these should be collected dynamically (e.g., asking the user for their email/phone via Telegram, or connecting a form).

## 💡 What I Learned

- How to trigger a workflow from a Telegram bot
- How to use the **Set** node to reshape and rename incoming data
- How to reference data from an earlier node using `$('Node Name').item.json` (used to grab the Telegram chat ID and message later in the flow)
- How to append rows to Google Sheets from n8n
- How to send emails automatically with Gmail
- How to send a reply back to a Telegram chat to confirm an action was completed

## 🚀 Possible Improvements (Future Me, Take Note)

- [ ] Replace hardcoded `email` and `phone number` with real user input
- [ ] Add validation/error handling (e.g., what happens if the sheet append fails?)
- [ ] Add a check for message type (currently assumes every update is a plain text message)
- [ ] Store data in a database instead of Google Sheets for scalability
- [ ] Add logging/notifications if something fails

## 📁 About This Repo

This repository is where I'll be saving and documenting my n8n workflows as I learn. Each project will include:
- The exported workflow JSON
- A README explaining what it does and what I learned

---
*Built while learning n8n — feedback and suggestions welcome!*
