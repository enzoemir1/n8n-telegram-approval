<p align="center">
  <img src="https://img.shields.io/badge/workflows-2-00D9FF?style=for-the-badge" alt="2 Workflows">
  <img src="https://img.shields.io/badge/platform-n8n-FF6D5A?style=for-the-badge" alt="n8n">
  <img src="https://img.shields.io/badge/bot-Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram">
  <img src="https://img.shields.io/badge/AI-OpenAI%20gpt--4o--mini-10B981?style=for-the-badge" alt="OpenAI">
  <img src="https://img.shields.io/badge/license-MIT-FFD700?style=for-the-badge" alt="MIT">
  <img src="https://img.shields.io/github/stars/enzoemir1/n8n-telegram-approval?style=for-the-badge&color=FFD700" alt="Stars">
</p>

<h1 align="center">n8n Telegram Approval</h1>

<p align="center">
  <strong>Add human-in-the-loop approval to any n8n workflow via Telegram.</strong><br/>
  Two ready-to-import workflows: one for AI content pipelines, one for generic approval of any data.
</p>

---

## Available Workflows

| Workflow | Description | Nodes | Cost |
|----------|-------------|-------|------|
| **[Content Approval](workflows/telegram-content-approval.json)** | Blog post → AI generates Twitter thread + LinkedIn post in parallel → Telegram approval | 10 | ~$0.003/run |
| **[Simple Approval](workflows/telegram-simple-approval.json)** | Any webhook data → Telegram Approve/Reject → Routes accordingly | 6 | Free |

---

## How It Works

```
                    Webhook receives
                    blog post / data
                           |
                    OpenAI generates
                    Twitter + LinkedIn        <-- Content workflow only
                    (parallel)
                           |
                    Telegram sends
                    content + buttons
                    [Approve] [Reject]
                           |
                    n8n Wait node
                    pauses workflow
                           |
                    User taps button
                      /          \
                Approve          Reject
                → post           → log
```

---

## Quick Start

```bash
# 1. Clone
git clone https://github.com/enzoemir1/n8n-telegram-approval.git

# 2. Import into n8n
#    Workflows → Import from File → select a JSON from workflows/

# 3. Add credentials
#    - Telegram Bot API token (from BotFather)
#    - OpenAI API key (content workflow only)

# 4. Set your Telegram Chat ID in the Telegram node

# 5. Activate and test
curl -X POST https://your-n8n/webhook/content-approval \
  -H "Content-Type: application/json" \
  -d '{"content": "Your blog post text here..."}'
```

---

## Telegram Bot Setup

1. Open Telegram and start a chat with **@BotFather**
2. Send `/newbot` and follow the prompts to create your bot
3. Copy the **Bot Token** — you need it for the n8n credential
4. Start a chat with your new bot (send any message)
5. Get your **Chat ID**:

```bash
curl https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates
# Find "chat":{"id": YOUR_CHAT_ID} in the response
```

6. Add the Chat ID to the Telegram node in the workflow

**Tip:** For team approvals, create a Telegram group, add the bot as admin, and use the group Chat ID.

---

## Customization

### Change AI Platforms
Edit the system prompts in the OpenAI nodes to generate content for different platforms (Instagram, Facebook, TikTok, Newsletter, etc.).

### Swap Telegram for Slack
Replace the Telegram node with Slack "Send Message" node using interactive buttons. Map the button callbacks to the same Wait node.

### Add Regeneration Loop
On the Reject path, route content back to the OpenAI node with feedback:

```
You previously generated: "{{original}}"
Feedback: "{{feedback}}"
Please rewrite, addressing the feedback.
```

### Change AI Model
Swap `gpt-4o-mini` for any OpenAI-compatible model. The workflow works with any provider that supports the chat completions API.

---

## Use Cases

- **Content publishing** — approve AI-generated social posts before they go live
- **Invoice validation** — route incoming invoices to a manager for quick approval
- **Deployment gates** — require manual confirmation before CI/CD proceeds
- **Lead qualification** — review and approve AI-scored leads before outreach
- **Support escalation** — let supervisors approve ticket escalations in real time
- **Data quality** — human review of AI-generated data transformations

---

## Requirements

| Requirement | Details |
|-------------|---------|
| **n8n** | Cloud or self-hosted (v1.0+) |
| **Telegram Bot** | Free via @BotFather |
| **OpenAI API** | Content workflow only · gpt-4o-mini · ~$0.003/run |

---

## Also by Automatia BCN

- **[autoflow-n8n-workflows](https://github.com/enzoemir1/autoflow-n8n-workflows)** — 8 free AI automation workflows for n8n
- **[free-ai-prompts](https://github.com/enzoemir1/free-ai-prompts)** — 90 free AI prompts for ChatGPT, Gemini & more
- **[All Products](https://automatiabcn.gumroad.com)** — Premium n8n workflows & SaaS boilerplates

---

## License

MIT License — free for personal and commercial use.

---

<p align="center">
  <strong>If this saves you time, a ⭐ would mean a lot!</strong>
</p>
