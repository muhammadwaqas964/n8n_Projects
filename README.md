# n8n AI DevOps Agent — Fiverr Client Manager

An AI-powered agent that automatically qualifies Fiverr clients, gathers complete DevOps project requirements, generates step-by-step implementation plans, sends Discord notifications, and creates GitHub issues — all automatically.

---

## What This Agent Does

- Greets Fiverr clients professionally
- Asks smart technical questions to gather all project requirements
- Remembers the entire conversation
- Waits for client confirmation before generating a plan
- Generates a complete step-by-step DevOps project plan
- Sends you a Discord notification instantly
- Creates a GitHub issue with full project details automatically

---

## Quick Start — Clone and Run in 10 Minutes

### Step 1 — Clone this repository

```bash
git clone https://github.com/muhammadwaqas964/n8n_Projects.git
cd n8n_Projects
```

### Step 2 — Install n8n

```bash
sudo npm install -g n8n
```

### Step 3 — Start n8n

```bash
n8n start
```

Open your browser and go to:
```
http://localhost:5678
```

Create an account when prompted.

### Step 4 — Get your free Gemini API key

1. Go to **https://aistudio.google.com**
2. Sign in with Google account
3. Click **"Get API Key"** → **"Create API key"**
4. Copy and save it

Verify it works:
```bash
curl "https://generativelanguage.googleapis.com/v1beta/models?key=YOUR_API_KEY"
```

If you get a list of models — you are good.

### Step 5 — Add Gemini credential in n8n

1. Open n8n at `http://localhost:5678`
2. Go to **Credentials** in left sidebar
3. Click **"Add Credential"**
4. Search for **"Google Gemini"**
5. Paste your API key
6. Click **Save**

### Step 6 — Create Discord webhook

1. Open Discord
2. Create a text channel (example: `fiverr-notifications`)
3. Click gear icon next to channel → **Integrations** → **Webhooks**
4. Click **"New Webhook"** → **"Copy Webhook URL"**
5. Save the URL

> Never share your webhook URL publicly.

### Step 7 — Create GitHub Personal Access Token

1. Go to **GitHub → Settings → Developer Settings**
2. Click **Personal Access Tokens → Tokens (classic)**
3. Click **"Generate new token (classic)"**
4. Name: `n8n-agent`
5. Expiration: **No expiration**
6. Check **"repo"** permission only
7. Click **Generate token** and copy it immediately

> Never share your token publicly. Treat it like a password.

### Step 8 — Edit the workflow file

Open `Fiverr_DevOps_Agent_v3.json` in any text editor and replace the two placeholders:

| Placeholder | Replace With |
|---|---|
| `PASTE_YOUR_DISCORD_WEBHOOK_URL_HERE` | Your Discord webhook URL |
| `PASTE_YOUR_GITHUB_TOKEN_HERE` | Your GitHub token |

Save the file.

### Step 9 — Import workflow into n8n

1. Open n8n at `http://localhost:5678`
2. Click **"+"** to create new workflow
3. Click **three dots (...)** top right
4. Click **"Import from file"**
5. Select the edited `Fiverr_DevOps_Agent_v3.json`
6. Double click **Google Gemini Chat Model** node
7. Select your saved Gemini credential
8. Click **Save**
9. Click **Publish**

### Step 10 — Test it

1. Click **"Open chat"** at the bottom of the canvas
2. Type a test message:
```
Hi, I need a CI/CD pipeline setup on AWS using GitHub Actions
```
3. The agent should reply professionally and start asking questions
4. Check Discord — you should receive a notification
5. After a full conversation, check your GitHub repo issues

---

## How to Use Daily

```
1. Client messages you on Fiverr
2. Copy their message
3. Open n8n chat at http://localhost:5678
4. Paste message into chat
5. Copy agent reply
6. Paste reply back to Fiverr client
7. Repeat until client confirms all requirements
8. Agent generates full project plan
9. Discord notifies you automatically
10. GitHub issue created with full project details
11. Copy project plan and start working
```

---

## Workflow Architecture

```
Client Message (Fiverr)
        ↓
   [You copy-paste]
        ↓
  n8n Chat Trigger
        ↓
     AI Agent
   (Gemini Brain)
    ↙    ↓    ↘
Memory  Model  Tools
        ↓
Discord Notification
        ↓
  GitHub Issue
```

---

## Files in this Repository

| File | Description |
|---|---|
| `README.md` | This guide |
| `Fiverr_DevOps_Agent_v3.json` | Ready-to-import n8n workflow |

---

## Troubleshooting

**n8n won't start**
```bash
sudo lsof -i :5678
sudo kill -9 PID
n8n start
```

**Gemini not working**
- Check API key is correct
- Make sure model is set to `models/gemini-1.5-flash`

**Discord not sending**
- Regenerate webhook if shared publicly
- Make sure body content type is JSON

**GitHub issue not creating**
- Token format must be: `token ghp_YOURTOKEN`
- Repo name is case sensitive — check exact spelling
- Make sure token has `repo` permission

**Agent not remembering conversation**
- Simple Memory node must be connected to AI Agent
- Set Context Window Length to at least 10

---

## Requirements

| Tool | Version | Cost |
|---|---|---|
| Node.js | 18+ | Free |
| n8n | Latest | Free (self-hosted) |
| Google Gemini API | 1.5 Flash | Free |
| Discord | Any | Free |
| GitHub | Any | Free |

**Total cost: $0**

---

## Author

**Muhammad Waqas** — DevOps & Cloud Engineer
- AWS Certified
- Azure Certified
- Skills: Docker, Kubernetes, Terraform, CI/CD, Linux, AWS, Azure

---

## License

MIT License — Free to use, modify, and share.
