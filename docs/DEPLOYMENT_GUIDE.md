# ChinaNewsStream Deployment Guide

Complete guide for all deployment methods with step-by-step instructions.

---

## 📋 Table of Contents

1. [Docker Deployment](#docker-deployment-recommended) ⭐ Recommended
2. [GitHub Actions Deployment](#github-actions-deployment)
3. [Local Python Deployment](#local-python-deployment)
4. [MCP Server Deployment](#mcp-server-deployment)

---

## 🐳 Docker Deployment (Recommended)

**Best for:** Production use, stable environment, automated scheduling

### Prerequisites

- Docker installed ([Get Docker](https://docs.docker.com/get-docker/))
- Docker Compose installed (usually included with Docker Desktop)

### Step 1: Clone Repository

```bash
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream
```

### Step 2: Configure Environment

```bash
# Copy environment template
cp .env.example .env

# Edit with your settings
nano .env  # or use your preferred editor
```

**Required settings in `.env`:**

```bash
# Enable features
ENABLE_CRAWLER=true
ENABLE_NOTIFICATION=true

# Choose push mode
REPORT_MODE=daily  # Options: daily, incremental, current

# Add at least one notification channel
FEISHU_WEBHOOK_URL=https://...
# OR
TELEGRAM_BOT_TOKEN=your_token
TELEGRAM_CHAT_ID=your_chat_id
# OR
EMAIL_FROM=your@email.com
EMAIL_PASSWORD=your_app_password
EMAIL_TO=recipient@email.com
```

### Step 3: Configure Keywords

Edit `config/frequency_words.txt` with topics you want to monitor:

```bash
nano config/frequency_words.txt
```

**Example:**
```txt
Tesla
SpaceX
+launch
!rumor

Bitcoin
Ethereum
+price
```

### Step 4: Launch Container

```bash
cd docker
docker-compose up -d
```

### Step 5: Verify Deployment

```bash
# Check container status
docker ps

# View logs
docker logs -f trend-radar

# Check output
ls -la ../output/
```

### Scheduled Execution

By default, runs every 5 minutes. To change:

**In `.env`:**
```bash
CRON_SCHEDULE=*/30 * * * *  # Every 30 minutes
```

**Cron syntax guide:**
```
*/5 * * * *   # Every 5 minutes
0 */1 * * *   # Every hour
0 9 * * *     # Daily at 9:00 AM
0 9,21 * * *  # Twice daily (9 AM and 9 PM)
0 9 * * 1-5   # Weekdays at 9 AM
```

### Managing the Container

```bash
# Stop container
docker-compose down

# Restart container
docker-compose restart

# Update container
docker-compose pull
docker-compose up -d

# View logs
docker logs trend-radar

# Access container shell
docker exec -it trend-radar sh
```

### Troubleshooting Docker

**Container won't start:**
```bash
# Check logs
docker logs trend-radar

# Check docker-compose configuration
docker-compose config

# Rebuild container
docker-compose down
docker-compose up -d --build
```

**Permission errors:**
```bash
# Fix output directory permissions
sudo chmod -R 777 output/
```

**No notifications:**
```bash
# Verify environment variables
docker exec trend-radar env | grep WEBHOOK
docker exec trend-radar env | grep ENABLE_NOTIFICATION
```

---

## ⚡ GitHub Actions Deployment

**Best for:** Free hosting, automatic scheduling, no server required

### Prerequisites

- GitHub account
- Forked ChinaNewsStream repository

### Step 1: Fork Repository

1. Visit: https://github.com/Havokyn/ChinaNewsStream
2. Click **Fork** button (top right)
3. Select your account

### Step 2: Enable GitHub Actions

1. Go to your forked repo
2. Click **Actions** tab
3. Click **"I understand my workflows, enable them"**

### Step 3: Configure Secrets

Go to **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

Add these secrets (choose your notification channel):

| Secret Name | Value | Required |
|------------|-------|----------|
| `FEISHU_WEBHOOK_URL` | Your Feishu webhook | Optional |
| `DINGTALK_WEBHOOK_URL` | Your DingTalk webhook | Optional |
| `WEWORK_WEBHOOK_URL` | Your WeChat Work webhook | Optional |
| `TELEGRAM_BOT_TOKEN` | Your Telegram bot token | Optional |
| `TELEGRAM_CHAT_ID` | Your Telegram chat ID | Optional |
| `EMAIL_FROM` | Sender email | Optional |
| `EMAIL_PASSWORD` | Email password | Optional |
| `EMAIL_TO` | Recipient email | Optional |

**At least one notification channel is required.**

### Step 4: Configure Keywords

Edit `config/frequency_words.txt` in your fork:

1. Go to file in GitHub web interface
2. Click pencil icon to edit
3. Add your keywords
4. Commit changes

### Step 5: Configure Platforms & Settings

Edit `config/config.yaml`:

```yaml
# Choose push mode
report:
  mode: "daily"  # daily, incremental, or current

# Adjust weights if needed
weight:
  rank_weight: 0.6
  frequency_weight: 0.3
  hotness_weight: 0.1

# Select platforms
platforms:
  - id: "zhihu"
    name: "Zhihu"
  - id: "weibo"
    name: "Weibo"
  # ... add or remove platforms
```

### Step 6: Adjust Schedule

Edit `.github/workflows/main.yml`:

```yaml
on:
  schedule:
    - cron: '0 * * * *'  # Every hour (default)
    # Change to your preference:
    # - cron: '*/30 * * * *'  # Every 30 minutes
    # - cron: '0 */2 * * *'   # Every 2 hours
```

### Step 7: Enable GitHub Pages (Optional)

For web reports:

1. **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **gh-pages** (will be created automatically)
4. Click **Save**

Access at: `https://your-username.github.io/ChinaNewsStream/`

### Monitoring GitHub Actions

**View runs:**
1. **Actions** tab
2. Click on workflow run
3. View logs and output

**Manual trigger:**
1. **Actions** tab
2. Select workflow
3. **Run workflow** button

### Troubleshooting GitHub Actions

**Workflow not running:**
- Check if Actions are enabled
- Verify cron syntax
- Check workflow file for errors

**No notifications:**
- Verify secrets are set correctly
- Check workflow logs for errors
- Test webhooks manually

**Rate limiting:**
- GitHub Actions has usage limits
- Free tier: 2000 minutes/month
- Reduce frequency if needed

---

## 🐍 Local Python Deployment

**Best for:** Development, testing, full control

### Prerequisites

- Python 3.10 or higher
- Git (optional)

### Step 1: Get Project

**Option A: Git clone**
```bash
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream
```

**Option B: Download ZIP**
1. Download from GitHub
2. Extract to folder
3. `cd` into folder

### Step 2: Run Setup Script

**macOS/Linux:**
```bash
chmod +x setup-mac.sh
./setup-mac.sh
```

**Windows:**
```cmd
setup-windows.bat
```

**Manual setup:**
```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install dependencies
uv sync
```

### Step 3: Configure

**Create `.env` file:**
```bash
cp .env.example .env
nano .env
```

**Edit `config/frequency_words.txt`** with your keywords

### Step 4: Run Crawler

```bash
# Single run
python main.py

# Or with uv
uv run python main.py
```

### Step 5: Automate Execution (Optional)

**macOS/Linux - Using cron:**

```bash
# Edit crontab
crontab -e

# Add line (runs every hour):
0 * * * * cd /path/to/ChinaNewsStream && /usr/local/bin/python main.py

# Or with uv:
0 * * * * cd /path/to/ChinaNewsStream && /path/to/uv run python main.py
```

**Windows - Using Task Scheduler:**

1. Open Task Scheduler
2. Create Basic Task
3. Trigger: Daily, repeat every 1 hour
4. Action: Start a program
5. Program: `python.exe`
6. Arguments: `main.py`
7. Start in: `C:\path\to\ChinaNewsStream`

---

## 🤖 MCP Server Deployment

**Best for:** AI-powered analysis with Claude, Cherry Studio, etc.

See dedicated [MCP Setup Guide](./MCP_SETUP_GUIDE.md) for detailed instructions.

### Quick Start

```bash
# Run setup
./setup-mac.sh  # or setup-windows.bat

# Start server
uv run python -m mcp_server.server

# For HTTP mode
uv run python -m mcp_server.server --transport http --port 3333
```

---

## 📊 Comparing Deployment Methods

| Feature | Docker | GitHub Actions | Local Python | MCP Server |
|---------|--------|----------------|--------------|------------|
| **Difficulty** | Easy | Medium | Medium | Hard |
| **Cost** | Free (self-hosted) | Free | Free | Free |
| **Automation** | ✅ Yes | ✅ Yes | Manual | Manual |
| **Scheduling** | Cron | Cron | Cron/Task Scheduler | N/A |
| **Reliability** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Customization** | High | Medium | High | High |
| **AI Integration** | No | No | No | ✅ Yes |
| **Web Dashboard** | ✅ Yes | ✅ Yes (Pages) | ✅ Yes | No |
| **Best For** | Production | Hobbyists | Developers | Analysis |

---

## 🔐 Security Best Practices

### Environment Variables

✅ **DO:**
- Store secrets in `.env` file
- Use GitHub Secrets for Actions
- Use environment variables in Docker

❌ **DON'T:**
- Commit `.env` to git
- Put webhooks in `config/config.yaml` in public repos
- Share credentials publicly

### Webhooks

✅ **DO:**
- Use HTTPS webhooks only
- Rotate tokens periodically
- Test webhooks before production
- Use app-specific passwords for email

❌ **DON'T:**
- Share webhook URLs publicly
- Use main account passwords
- Ignore webhook expiration notices

### Docker

✅ **DO:**
- Use read-only mounts where possible
- Keep images updated
- Limit container resources

❌ **DON'T:**
- Run as root unnecessarily
- Expose unnecessary ports
- Ignore security updates

---

## 🆘 Getting Help

**Common Issues:**

| Problem | Solution |
|---------|----------|
| No notifications | Check webhook URLs, verify `ENABLE_NOTIFICATION=true` |
| No data | Check `ENABLE_CRAWLER=true`, verify network connection |
| Permission errors | Fix with `chmod -R 777 output/` |
| Python version | Upgrade to Python 3.10+ |
| Docker won't start | Check logs with `docker logs trend-radar` |

**Resources:**
- [GitHub Issues](https://github.com/Havokyn/ChinaNewsStream/issues)
- [Original Project](https://github.com/sansan0/TrendRadar)
- [MCP Protocol](https://modelcontextprotocol.io/)

---

## ✅ Deployment Checklist

### Pre-Deployment
- [ ] Python 3.10+ installed (if not using Docker)
- [ ] Webhook URLs obtained
- [ ] Keywords configured
- [ ] Platforms selected
- [ ] Push mode chosen

### Post-Deployment
- [ ] Container/process running
- [ ] First crawl successful
- [ ] Notifications received
- [ ] Output files generated
- [ ] Web dashboard accessible (if enabled)

### Maintenance
- [ ] Monitor logs weekly
- [ ] Check disk space monthly
- [ ] Update dependencies quarterly
- [ ] Rotate credentials annually

---

**You're all set!** Choose the deployment method that fits your needs and start monitoring Chinese news trends. 📰
