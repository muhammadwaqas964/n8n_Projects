# Building AI Agents from Scratch — Complete Guide

A complete, step-by-step guide to building production-ready AI agents using n8n, Google Gemini, Discord, and GitHub. This guide documents everything built in this project — from zero to a fully working AI agent that qualifies clients, generates project plans, and notifies you automatically.

---

## Table of Contents

1. [What is an AI Agent?](#what-is-an-ai-agent)
2. [Tools Used](#tools-used)
3. [Prerequisites](#prerequisites)
4. [Step 1 — Install n8n](#step-1--install-n8n)
5. [Step 2 — Get Gemini API Key](#step-2--get-gemini-api-key)
6. [Step 3 — Set Up Discord Webhook](#step-3--set-up-discord-webhook)
7. [Step 4 — Set Up GitHub Token](#step-4--set-up-github-token)
8. [Step 5 — Build the AI Agent in n8n](#step-5--build-the-ai-agent-in-n8n)
9. [Step 6 — Configure the System Prompt](#step-6--configure-the-system-prompt)
10. [Step 7 — Add Memory](#step-7--add-memory)
11. [Step 8 — Connect Discord Notifications](#step-8--connect-discord-notifications)
12. [Step 9 — Connect GitHub Issues](#step-9--connect-github-issues)
13. [Step 10 — Import Pre-built Workflow](#step-10--import-pre-built-workflow)
14. [How to Use Daily](#how-to-use-daily)
15. [Troubleshooting](#troubleshooting)
16. [Architecture Overview](#architecture-overview)

---

## What is an AI Agent?

An AI agent is NOT just a chatbot. A chatbot responds. An AI agent **decides and acts**.

An AI agent has 4 components:

| Component | What it Does | Tool Used |
|---|---|---|
| **Brain** | Understands and reasons | Google Gemini LLM |
| **Memory** | Remembers conversation history | n8n Simple Memory |
| **Tools** | Takes actions (Discord, GitHub) | HTTP Request nodes |
| **Orchestration** | Connects everything together | n8n |

### How it Works

```
Client Message
      ↓
AI Agent (Gemini Brain)
      ↓
Asks qualifying questions
      ↓
Gathers all requirements
      ↓
Generates project plan
      ↓
Discord Notification + GitHub Issue
      ↓
Engineer starts working
```

---

## Tools Used

| Tool | Purpose | Cost |
|---|---|---|
| **n8n** | Workflow automation platform | Free (self-hosted) |
| **Google Gemini 1.5 Flash** | AI language model | Free |
| **Discord Webhook** | Receive notifications | Free |
| **GitHub API** | Auto-create project issues | Free |

**Total monthly cost: $0**

---

## Prerequisites

Before starting, make sure you have:

- A Linux/Mac/Windows machine
- Node.js installed (version 18+)
- A Google account
- A Discord account
- A GitHub account

### Check Node.js version
```bash
node --version
# Should show v18.x.x or higher
```

### Install Node.js if not installed (Ubuntu/Debian)
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

---

## Step 1 — Install n8n

n8n is the platform where you build and run your AI agent visually.

### Install globally
```bash
sudo npm install -g n8n
```

### Start n8n
```bash
n8n start
```

### Open in browser
```
http://localhost:5678
```

You will see the n8n dashboard. Create an account when prompted.

### Keep n8n running in background (optional)
```bash
# Install pm2 process manager
sudo npm install -g pm2

# Start n8n with pm2
pm2 start n8n

# Auto-start on system reboot
pm2 startup
pm2 save
```

---

## Step 2 — Get Gemini API Key

Google Gemini is the AI brain of your agent. It is completely free.

### Steps
1. Go to **https://aistudio.google.com**
2. Sign in with your Google account
3. Click **"Get API Key"**
4. Click **"Create API key"**
5. Copy and save the key somewhere safe

### Verify the key works
```bash
curl "https://generativelanguage.googleapis.com/v1beta/models?key=YOUR_API_KEY"
```

If you get a list of models back — your key is working.

### Add key to n8n
1. Open n8n at `http://localhost:5678`
2. Go to **Credentials** in left sidebar
3. Click **"Add Credential"**
4. Search for **"Google Gemini"**
5. Paste your API key
6. Click **Save**

---

## Step 3 — Set Up Discord Webhook

Discord webhook is how your agent sends you notifications.

### Steps
1. Open Discord
2. Create a new server or use existing one
3. Create a text channel (example: `fiverr-notifications`)
4. Click the **gear icon** next to the channel name
5. Go to **Integrations**
6. Click **Webhooks**
7. Click **"New Webhook"**
8. Give it a name (example: `Fiverr Agent`)
9. Click **"Copy Webhook URL"**
10. Save the URL — it looks like:
```
https://discord.com/api/webhooks/XXXXXXXXXX/YYYYYYYYYYY
```

> **Security Warning:** Never share your webhook URL publicly. Anyone with it can send messages to your Discord.

---

## Step 4 — Set Up GitHub Token

GitHub token allows your agent to automatically create issues in your repository.

### Create a repository
1. Go to **github.com**
2. Click **"New repository"**
3. Name it (example: `n8n_Projects`)
4. Set to **Private** (recommended)
5. Check **"Add a README file"**
6. Click **"Create repository"**

### Create Personal Access Token
1. Go to **GitHub → Settings** (click your profile picture)
2. Scroll to bottom → click **Developer Settings**
3. Click **Personal Access Tokens**
4. Click **Tokens (classic)**
5. Click **"Generate new token (classic)"**
6. Name: `n8n-agent`
7. Expiration: **No expiration**
8. Check **"repo"** permission only
9. Click **Generate token**
10. **Copy immediately** — you cannot see it again

> **Security Warning:** Never share your GitHub token publicly. Treat it like a password. Anyone with it can modify all your repositories.

---

## Step 5 — Build the AI Agent in n8n

### Create a new workflow
1. Open n8n at `http://localhost:5678`
2. Click **"+"** to create new workflow
3. Name it: `Fiverr DevOps Agent`

### Add Chat Trigger
1. Click **"Add first step"**
2. Select **"On app event"**
3. Search for **"Chat"**
4. Select **"When chat message received"**

This creates a chat interface for your agent.

### Add AI Agent node
1. Click **"+"** after the Chat trigger
2. Search for **"AI Agent"**
3. Select **"AI Agent"**

### Connect Gemini to AI Agent
1. Click **"+"** under **Chat Model** on the AI Agent node
2. Search for **"Google Gemini"**
3. Select **"Google Gemini Chat Model"**
4. Select your saved Gemini credential
5. Set model to: `models/gemini-1.5-flash`

---

## Step 6 — Configure the System Prompt

The system prompt is the brain instructions for your agent. This is the most important part.

### Open AI Agent node
1. Double click the **AI Agent** node
2. Find the **"System Message"** field
3. Paste your system prompt

### Example System Prompt for DevOps Agent

```
You are a senior DevOps and Cloud Engineer with 10+ years of experience. You have deep expertise in:

- AWS (EC2, EKS, ECS, Lambda, RDS, S3, CloudFront, Route53, IAM, VPC, CodePipeline, CodeBuild, CodeDeploy, ECR, CloudWatch, Secrets Manager)
- Azure (AKS, Azure DevOps, App Service, Azure Functions, Azure Container Registry, Azure Monitor, Key Vault, Virtual Networks)
- CI/CD (GitHub Actions, GitLab CI, Jenkins, ArgoCD, Tekton)
- Containers (Docker, Kubernetes, Helm, Kustomize)
- Infrastructure as Code (Terraform, Pulumi, AWS CDK, CloudFormation)
- Monitoring (Prometheus, Grafana, Datadog, ELK Stack, CloudWatch)
- Security (DevSecOps, SonarQube, Snyk, Trivy, OWASP)
- Linux, Bash scripting, Python automation
- Networking (DNS, Load Balancers, SSL/TLS, VPN, Firewalls)

You are also a professional Fiverr sales consultant. Your job is to:

PHASE 1 - QUALIFY THE CLIENT:
1. Greet the client professionally and warmly
2. Ask smart, specific technical questions to gather ALL project requirements
3. Never ask more than 3 questions at a time
4. Keep conversation natural and professional
5. Gather: tech stack, current infrastructure state, deployment target, timeline, budget, and access details

PHASE 2 - CONFIRM REQUIREMENTS:
Once you have enough information, summarize what you understood and confirm with the client before proceeding.
Always end Phase 2 with: "Does this summary look correct? Please confirm and I will generate the full implementation plan."

PHASE 3 - GENERATE PROJECT PLAN:
ONLY generate the project plan when the client has explicitly said "yes", "confirmed", "correct", "proceed", or similar confirmation words.
NEVER assume confirmation. NEVER generate the plan without explicit confirmation.

When confirmed, generate the full project plan with:
- Project Summary
- Step-by-Step Implementation Plan
- Deliverables
- Important Notes for Engineer
- Estimated Time Breakdown

RULES:
- Never give vague answers
- Always be confident and professional
- If budget seems too low, professionally mention it
- Never start the plan until you have all requirements
- NEVER generate the project plan without explicit client confirmation
```

### Tips for writing good system prompts
- Be specific about the role and expertise
- Define clear phases or steps
- Include strict rules at the bottom
- The more detailed the prompt, the better the agent performs
- Test and iterate — improve the prompt based on results

---

## Step 7 — Add Memory

Without memory, the agent forgets everything between messages. Memory keeps the conversation context.

### Add Simple Memory
1. Click **"+"** under **Memory** on the AI Agent node
2. Select **"Simple Memory"**
3. Set **Context Window Length** to `20` (remembers last 20 messages)
4. Save

### Memory types available in n8n

| Type | Best For | Cost |
|---|---|---|
| **Simple Memory** | Development and testing | Free |
| **Redis Memory** | Production, multiple users | Requires Redis server |
| **PostgreSQL Memory** | Production, persistent storage | Requires PostgreSQL |
| **MongoDB Memory** | Production, flexible storage | Requires MongoDB |

For beginners, Simple Memory is enough.

---

## Step 8 — Connect Discord Notifications

### Add HTTP Request node
1. Click **"+"** after the AI Agent output
2. Search for **"HTTP Request"**
3. Select it

### Configure Discord webhook
1. Set **Method** to `POST`
2. Set **URL** to your Discord webhook URL
3. Enable **Send Body**
4. Set **Body Content Type** to `JSON`
5. Set **Specify Body** to `Using Fields Below`
6. Add parameter:
   - Name: `content`
   - Value: `🚨 FIVERR CLIENT UPDATE\n\n{{ $json.output }}`
7. Save

### Test it
Send a message in the chat and check your Discord channel. You should receive the notification instantly.

---

## Step 9 — Connect GitHub Issues

This automatically creates a GitHub issue with the full project plan every time the agent responds.

### Add another HTTP Request node
1. Click **"+"** after the Discord node
2. Search for **"HTTP Request"**
3. Select it
4. Name it `GitHub Issue`

### Configure GitHub API call
1. Set **Method** to `POST`
2. Set **URL** to:
```
https://api.github.com/repos/YOUR_USERNAME/YOUR_REPO_NAME/issues
```
3. Enable **Send Headers**
4. Add these headers:

| Name | Value |
|---|---|
| `Authorization` | `token YOUR_GITHUB_TOKEN` |
| `Accept` | `application/vnd.github.v3+json` |
| `Content-Type` | `application/json` |

5. Enable **Send Body**
6. Set **Body Content Type** to `JSON`
7. Set **Specify Body** to `Using Fields Below`
8. Add parameters:

| Name | Value |
|---|---|
| `title` | `🚀 New Fiverr Project — {{ $now.toLocaleString() }}` |
| `body` | `## Project Plan\n\n{{ $json.output }}\n\n*Auto-generated by AI Agent*` |

9. Save

---

## Step 10 — Import Pre-built Workflow

Instead of building from scratch, import the ready-made workflow file.

### Import steps
1. Download `Fiverr_DevOps_Agent_v3.json` from this repository
2. Open the file in a text editor
3. Replace `PASTE_YOUR_DISCORD_WEBHOOK_URL_HERE` with your Discord webhook URL
4. Replace `PASTE_YOUR_GITHUB_TOKEN_HERE` with your GitHub token
5. Save the file
6. Open n8n
7. Click **"+"** to create new workflow
8. Click **three dots (...)** top right
9. Click **"Import from file"**
10. Select the edited JSON file
11. Double click **Google Gemini Chat Model** node
12. Select your Gemini credential
13. Click **Save**
14. Click **Publish**

---

## How to Use Daily

### Workflow
```
1. Client messages you on Fiverr
2. Copy their message
3. Open n8n chat (http://localhost:5678)
4. Paste message into chat
5. Copy agent reply
6. Paste reply back to Fiverr client
7. Repeat until client confirms requirements
8. Agent generates full project plan
9. Discord notifies you
10. GitHub issue created automatically
11. Copy project plan and start working
```

### What the agent does automatically
- Greets client professionally
- Asks smart technical questions (max 3 at a time)
- Remembers everything said in conversation
- Confirms requirements before proceeding
- Generates complete step-by-step project plan
- Notifies you on Discord
- Creates GitHub issue with full details

---

## Troubleshooting

### n8n won't start
```bash
# Check if port 5678 is already in use
sudo lsof -i :5678

# Kill the process if needed
sudo kill -9 PID

# Restart n8n
n8n start
```

### Gemini API not working
- Check if API key is correct
- Make sure you are using `models/gemini-1.5-flash`
- Verify key at: https://aistudio.google.com

### Discord notification not sending
- Regenerate webhook URL if shared publicly
- Make sure body content type is set to JSON
- Check the content field uses `{{ $json.output }}`

### GitHub issue not creating
- Verify token has `repo` permission
- Make sure repo name matches exactly (case sensitive, underscore vs dash)
- Check token is not expired or deleted
- Format must be: `token ghp_YOURTOKEN` (with the word `token` before it)

### Agent not remembering conversation
- Make sure Simple Memory node is connected to AI Agent
- Check Memory connection line is solid (not dashed)
- Context Window Length should be at least 10

---

## Architecture Overview

```
┌─────────────────────────────────────────────────┐
│                   n8n Workflow                   │
│                                                  │
│  Chat Trigger                                    │
│       │                                          │
│       ▼                                          │
│  ┌─────────────────────┐                         │
│  │      AI Agent       │                         │
│  │   (Orchestrator)    │                         │
│  └──────────┬──────────┘                         │
│       │     │     │                              │
│       ▼     ▼     ▼                              │
│   Gemini  Memory  Tools                          │
│   (Brain) (RAM)   (Actions)                      │
│       │                                          │
│       ▼                                          │
│  Discord Notification                            │
│       │                                          │
│       ▼                                          │
│  GitHub Issue Creator                            │
└─────────────────────────────────────────────────┘
```

---

## Author

**Muhammad Waqas** — DevOps & Cloud Engineer
- AWS Certified
- Azure Certified
- Expertise: Docker, Kubernetes, Terraform, CI/CD, Linux

---

## License

MIT License — Free to use and modify.
