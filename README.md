# ChinaNewsStream

<div align="center">

![Banner](/_image/banner.jpg)

**Deploy in 30 seconds** - Your intelligent Chinese news aggregation assistant

Monitor trending topics from 11+ major Chinese platforms with AI-powered filtering

[![GitHub Stars](https://img.shields.io/github/stars/Havokyn/ChinaNewsStream?style=flat-square&logo=github&color=yellow)](https://github.com/Havokyn/ChinaNewsStream/stargazers)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/version-v3.0.5-blue.svg)](https://github.com/Havokyn/ChinaNewsStream)

</div>

> **Fork Information:** This is an English fork of [TrendRadar](https://github.com/sansan0/TrendRadar) by sansan0. All functionality preserved, documentation translated to English.

---

## 📑 Table of Contents

| [🎯 Features](#-features) | [🚀 Quick Start](#-quick-start) | [🐳 Docker](#-docker-deployment) | [🤖 AI Analysis](#-ai-powered-analysis) |
|:---:|:---:|:---:|:---:|
| [📝 Configuration](#-configuration) | [🔔 Notifications](#-notification-channels) | [❓ FAQ](#-troubleshooting) | [📄 License](#-license) |

---

## ✨ Features

### Multi-Platform News Aggregation

Monitor trending topics from 11+ major Chinese platforms:

- **Zhihu** (知乎) - Q&A platform
- **Weibo** (微博) - Microblogging
- **Douyin** (抖音) - Short video
- **Bilibili** - Video sharing
- **Baidu** (百度) - Search trends
- **Toutiao** (今日头条) - News aggregator
- **Tieba** (贴吧) - Forums
- **Phoenix News** (凤凰网)
- **The Paper** (澎湃新闻)
- **Wallstreet CN** (华尔街见闻)
- **Cailian Press** (财联社)

**Customize your platforms** - Add more from [newsnow sources](https://github.com/ourongxing/newsnow/tree/main/server/sources)

### Intelligent Push Strategies

Three notification modes to match your needs:

| Mode | Best For | Timing | Content | Use Case |
|------|----------|--------|---------|----------|
| **Daily Summary**<br/>`daily` | General users | Scheduled (hourly) | All matching news today<br/>+ New items section | Daily briefings, trend overview |
| **Current Trending**<br/>`current` | Content creators | Scheduled (hourly) | Current trending matches<br/>+ New items section | Real-time hot topics |
| **Incremental**<br/>`incremental` | Investors/Traders | Only when new items appear | New matching items only | High-frequency monitoring |

**Optional: Push Time Windows**

Control when you receive notifications:
- Set active hours (e.g., 09:00-18:00 or 20:00-22:00)
- Once-per-day summaries
- Avoid off-hours interruptions

### Precision Keyword Filtering

Define custom keywords to filter relevant news:

**Three syntax types:**

| Type | Symbol | Purpose | Example | Logic |
|------|--------|---------|---------|-------|
| **Normal** | none | Basic match | `Tesla` | Match any keyword |
| **Required** | `+` | Scope limiter | `+price` | Must include this |
| **Filter** | `!` | Exclude noise | `!rumor` | Exclude if present |

**Example configuration:**
```txt
Tesla
SpaceX
+launch
!advertisement

Apple
iPhone
+release
!leak

Bitcoin
Ethereum
+price
!prediction
```

**Matching logic:**
- ✅ "SpaceX launches new satellite" (has SpaceX + launch)
- ✅ "iPhone 15 release date confirmed" (has iPhone + release)
- ❌ "Tesla rumored to cut prices" (has Tesla but includes "rumor")
- ❌ "Apple reports earnings" (has Apple but missing "release")

### Trend Analysis & Tracking

Understand not just *what's trending*, but *how trends evolve*:

- **Timeline Tracking**: Full lifespan from first to last appearance
- **Heat Changes**: Ranking and frequency over time
- **New Detection**: 🆕 marker for fresh topics
- **Persistence Analysis**: Distinguish one-time spikes from sustained topics
- **Cross-Platform Comparison**: See how different platforms prioritize stories

### Personalized Ranking Algorithm

Stop being controlled by platform algorithms - ChinaNewsStream reranks news based on:

- **Ranking Weight** (60%): Top-ranked news gets priority
- **Frequency Weight** (30%): Repeatedly appearing topics are important
- **Hotness Weight** (10%): Quality of rankings matters

**Customizable weights** - Adjust for your use case:

```yaml
# Real-time trending (for content creators)
weight:
  rank_weight: 0.8
  frequency_weight: 0.1
  hotness_weight: 0.1

# Deep analysis (for investors/researchers)
weight:
  rank_weight: 0.4
  frequency_weight: 0.5
  hotness_weight: 0.1
```

### Multi-Channel Notifications

Push alerts to your preferred platform:

- **Enterprise WeChat** (企业微信)
- **Feishu/Lark** (飞书)
- **DingTalk** (钉钉)
- **Telegram**
- **Email** (SMTP)
- **ntfy** (self-hosted or ntfy.sh)

All notifications include clickable links and formatted content.

### Multi-Platform Deployment

- **GitHub Actions**: Free cloud automation
- **Docker**: Multi-architecture containers (amd64, arm64)
- **GitHub Pages**: Auto-generated web reports
- **Local Python**: Direct execution
- **Data Persistence**: HTML/TXT historical archives

### AI-Powered Analysis (v3.0.0+)

MCP (Model Context Protocol) integration for intelligent analysis:

**13 AI Tools:**
- Natural language queries: "Show yesterday's Zhihu trends"
- Topic trend analysis (heat changes, lifecycle, viral detection)
- Cross-platform comparisons
- Sentiment analysis
- Similar news detection
- Automated summary reports

**Supported Clients:**
- Cherry Studio (GUI)
- Claude Desktop
- Cursor IDE
- Cline
- Any MCP-compatible client

---

## 🚀 Quick Start

### Prerequisites

- Python 3.10 or higher
- `uv` package manager (auto-installed by setup scripts)

### Option 1: Automated Setup (Recommended)

**macOS/Linux:**
```bash
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream
chmod +x setup-mac.sh
./setup-mac.sh
```

**Windows:**
```cmd
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream
setup-windows.bat
```

### Option 2: Manual Setup

```bash
# Clone repository
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream

# Install uv (if not already installed)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install dependencies
uv sync

# Configure
cp .env.example .env
nano .env  # Edit with your settings
```

### Configuration

1. **Set up environment variables** (recommended):
```bash
cp .env.example .env
# Edit .env with your webhook URLs and settings
```

2. **Configure keywords**:
Edit `config/frequency_words.txt` with topics you want to monitor.

3. **Run the crawler**:
```bash
# Standalone mode
python main.py

# MCP server mode
uv run python -m mcp_server.server
```

---

## 🐳 Docker Deployment

### Quick Start with Docker Compose

```bash
# Clone repository
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream

# Configure environment
cp .env.example .env
nano .env  # Add your webhook URLs

# Edit keywords
nano config/frequency_words.txt

# Start container
cd docker
docker-compose up -d
```

### View Logs

```bash
docker logs -f trend-radar
```

### Stop Container

```bash
docker-compose down
```

### Docker Environment Variables

All settings can be controlled via environment variables:

```bash
# Core settings
ENABLE_CRAWLER=true
ENABLE_NOTIFICATION=true
REPORT_MODE=daily

# Notification webhooks
FEISHU_WEBHOOK_URL=https://...
TELEGRAM_BOT_TOKEN=your_token
TELEGRAM_CHAT_ID=your_chat_id

# Scheduling
CRON_SCHEDULE=*/5 * * * *  # Every 5 minutes
RUN_MODE=cron
```

See `.env.example` for complete list.

---

## 📝 Configuration

### config/config.yaml

Main configuration file for platforms, weights, and notification settings.

**Key sections:**

```yaml
# Crawler settings
crawler:
  request_interval: 1000  # Milliseconds between requests
  enable_crawler: true
  use_proxy: false
  default_proxy: "http://127.0.0.1:10086"

# Push mode
report:
  mode: "daily"  # Options: daily, incremental, current
  rank_threshold: 5  # Highlight threshold for rankings

# Notification settings
notification:
  enable_notification: true
  push_window:
    enabled: false
    time_range:
      start: "20:00"
      end: "22:00"
    once_per_day: true

# Ranking weights
weight:
  rank_weight: 0.6
  frequency_weight: 0.3
  hotness_weight: 0.1

# Monitored platforms
platforms:
  - id: "zhihu"
    name: "Zhihu"
  - id: "weibo"
    name: "Weibo"
  # Add more platforms...
```

### config/frequency_words.txt

Define keywords to monitor. Supports grouping and operators.

**Example:**

```txt
# Tech Companies
Tesla
SpaceX
+Elon Musk
!rumor

# Cryptocurrency
Bitcoin
Ethereum
+price
!prediction

# AI Technology
ChatGPT
Claude
Gemini
+AI
+artificial intelligence
!advertisement
```

**Syntax:**
- Blank lines separate groups
- Normal words: Any match triggers capture
- `+word`: Required word (must appear with normal words)
- `!word`: Filter word (excludes news containing this)

**Matching examples:**

✅ "SpaceX launches satellite with Elon Musk attending" (has SpaceX + Elon Musk, no rumor)
✅ "Bitcoin price surges to new high" (has Bitcoin + price, no prediction)
❌ "Tesla rumor about new factory" (has Tesla but includes rumor)
❌ "ChatGPT releases update" (has ChatGPT but missing AI/artificial intelligence)

---

## 🔔 Notification Channels

### Feishu/Lark

```bash
# Get webhook from Feishu bot settings
export FEISHU_WEBHOOK_URL="https://open.feishu.cn/open-apis/bot/v2/hook/..."
```

### DingTalk

```bash
# Get webhook from DingTalk bot settings
export DINGTALK_WEBHOOK_URL="https://oapi.dingtalk.com/robot/send?access_token=..."
```

### Enterprise WeChat

```bash
# Get webhook from Enterprise WeChat bot settings
export WEWORK_WEBHOOK_URL="https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=..."
```

### Telegram

```bash
# Create bot via @BotFather, get chat ID via @userinfobot
export TELEGRAM_BOT_TOKEN="your_bot_token"
export TELEGRAM_CHAT_ID="your_chat_id"
```

### Email (SMTP)

```bash
# Auto-detects SMTP settings for major providers
export EMAIL_FROM="your_email@gmail.com"
export EMAIL_PASSWORD="your_app_password"
export EMAIL_TO="recipient@example.com"

# Optional: Override SMTP settings
export EMAIL_SMTP_SERVER="smtp.gmail.com"
export EMAIL_SMTP_PORT="587"
```

**Supported providers:**
- Gmail (smtp.gmail.com:587 TLS)
- Outlook/Hotmail (smtp-mail.outlook.com:587 TLS)
- QQ Mail (smtp.qq.com:465 SSL)
- 163/126 Mail (smtp.163.com:465 SSL)

### ntfy

```bash
# Use public ntfy.sh or self-hosted server
export NTFY_SERVER_URL="https://ntfy.sh"
export NTFY_TOPIC="your_unique_topic"
export NTFY_TOKEN="optional_token_for_private_topics"
```

---

## 🤖 AI-Powered Analysis

### MCP Server Mode

ChinaNewsStream includes a Model Context Protocol (MCP) server for AI-powered analysis.

### Setup with Cherry Studio

1. **Start MCP server:**
```bash
./setup-mac.sh  # or setup-windows.bat
```

2. **Configure Cherry Studio:**
   - Settings > MCP Servers > Add Server
   - Name: ChinaNewsStream
   - Type: STDIO
   - Command: `/path/to/uv`
   - Arguments (one per line):
     ```
     --directory
     /path/to/ChinaNewsStream
     run
     python
     -m
     mcp_server.server
     ```

3. **Enable MCP** in Cherry Studio settings

### Available AI Tools (13 total)

**Data Query:**
- `get_latest_news` - Fetch recent trending news
- `get_news_by_date` - Query historical data (supports natural language: "yesterday", "3 days ago")
- `get_trending_topics` - Keyword frequency statistics

**Smart Search:**
- `search_news` - Unified search (keyword/fuzzy/entity modes)
- `search_related_news_history` - Find related historical news

**Advanced Analytics:**
- `analyze_topic_trend` - Topic trend analysis (heat/lifecycle/viral/prediction)
- `analyze_data_insights` - Data insights (platform comparison/activity/keyword co-occurrence)
- `analyze_sentiment` - Sentiment analysis
- `find_similar_news` - Similarity detection
- `generate_summary_report` - Automated daily/weekly summaries

**System Management:**
- `get_current_config` - View configuration
- `get_system_status` - System health check
- `trigger_crawl` - Manual crawl trigger

### Example AI Queries

```
"Show me yesterday's trending topics on Zhihu"
"Analyze the trend of Bitcoin over the last 7 days"
"Find news similar to 'Tesla price cut announcement'"
"Compare platform activity for AI-related topics"
"Generate a weekly summary report"
```

---

## 🔧 Advanced Configuration

### Push Time Windows

Limit notifications to specific hours:

```yaml
notification:
  push_window:
    enabled: true
    time_range:
      start: "09:00"  # Beijing time
      end: "18:00"
    once_per_day: true  # Only push once in window
    push_record_retention_days: 7
```

Environment variable override:
```bash
export PUSH_WINDOW_ENABLED=true
export PUSH_WINDOW_START="09:00"
export PUSH_WINDOW_END="18:00"
export PUSH_WINDOW_ONCE_PER_DAY=true
```

### Custom Platforms

Add platforms from [newsnow sources](https://github.com/ourongxing/newsnow/tree/main/server/sources):

```yaml
platforms:
  - id: "36kr"
    name: "36Kr Tech News"
  - id: "sspai"
    name: "SSPAI"
  - id: "hellogithub"
    name: "HelloGitHub"
```

### Proxy Configuration

```yaml
crawler:
  use_proxy: true
  default_proxy: "http://127.0.0.1:7890"
```

---

## ❓ Troubleshooting

### No notifications received

1. **Check environment variables:**
```bash
# Verify webhooks are set
echo $FEISHU_WEBHOOK_URL
echo $ENABLE_NOTIFICATION
```

2. **Check logs:**
```bash
# Docker
docker logs trend-radar

# Standalone
python main.py  # Check console output
```

3. **Test webhook manually:**
```bash
# Feishu example
curl -X POST "$FEISHU_WEBHOOK_URL" \
  -H 'Content-Type: application/json' \
  -d '{"msg_type":"text","content":{"text":"Test"}}'
```

### No data in output directory

1. **Verify crawler is enabled:**
```bash
export ENABLE_CRAWLER=true
```

2. **Check network connectivity:**
```bash
curl -I https://newsnow.busiyi.world/api/s?id=zhihu&latest
```

3. **Check proxy settings** if behind firewall

### Permission errors (Docker)

```bash
# Fix output directory permissions
sudo chmod -R 777 output/
```

### MCP server not connecting

1. **Verify uv installation:**
```bash
uv --version
```

2. **Check project path** in MCP client configuration

3. **Test server manually:**
```bash
uv run python -m mcp_server.server
# Should show server startup messages
```

---

## 📊 Output Formats

### Text Format (output/YYYY年MM月DD日/txt/)

Plain text with rankings and timestamps:
```
zhihu | Zhihu
1. AI breakthrough announced [URL:...] [MOBILE:...]
2. Tech company IPO news
...
```

### HTML Format (output/YYYY年MM月DD日/html/)

Responsive web pages with:
- Filtering controls
- Sorting options
- Mobile-friendly design
- Screenshot capability

### JSON Format

Structured data for programmatic access via MCP tools.

---

## 🚀 Deployment Options Comparison

| Method | Pros | Cons | Best For |
|--------|------|------|----------|
| **Docker** | Easy setup, isolated, cron scheduling | Requires Docker | Production use |
| **GitHub Actions** | Free hosting, automated | Public repo, rate limits | Personal projects |
| **Local Python** | Full control, debugging | Manual execution | Development |
| **MCP Server** | AI integration | Requires MCP client | Analysis tasks |

---

## 📄 License

This project is licensed under **GNU General Public License v3.0** (GPL-3.0).

### What this means:

✅ **You can:**
- Use for any purpose
- Study and modify the code
- Distribute copies
- Distribute modified versions

⚠️ **You must:**
- Keep derivative works open source under GPL-3.0
- Maintain copyright notices
- Document changes made
- Provide source code to users

### Attribution

This project is a fork of [TrendRadar](https://github.com/sansan0/TrendRadar) by sansan0.

Data is sourced from [newsnow](https://github.com/ourongxing/newsnow) project.

---

## 🙏 Acknowledgments

**Original Project:**
- sansan0 - [TrendRadar](https://github.com/sansan0/TrendRadar)
- ourongxing - [newsnow](https://github.com/ourongxing/newsnow) (data source)

**Referenced by:**
- [小众软件](https://mp.weixin.qq.com/s/fvutkJ_NPUelSW9OGK39aA) - Open source software platform
- [LinuxDo Community](https://linux.do/) - Tech enthusiast community
- [Ruan Yifeng's Weekly](https://github.com/ruanyf/weekly) - Influential tech newsletter

---

## 🤝 Contributing

This is a stable fork not actively synced with upstream. For contributions:

1. **Bug fixes** - Open an issue
2. **Feature requests** - Consider contributing to [original project](https://github.com/sansan0/TrendRadar)
3. **Documentation** - PRs welcome for English docs

---

## 📞 Support

- **Original Project Issues:** https://github.com/sansan0/TrendRadar/issues
- **Fork-Specific Issues:** https://github.com/Havokyn/ChinaNewsStream/issues
- **Data Source:** https://newsnow.busiyi.world/

---

## 🔐 Security Notes

### Best Practices:

1. **Never commit credentials:**
   - Use `.env` for all secrets
   - `.gitignore` is pre-configured to exclude sensitive files
   - Use environment variables in production

2. **Webhook security:**
   - Store in environment variables, not `config.yaml`
   - Use GitHub Secrets for GitHub Actions
   - Rotate tokens periodically

3. **SMTP passwords:**
   - Use app-specific passwords (not main account password)
   - Enable 2FA on email accounts

4. **Docker security:**
   - Run containers with limited permissions
   - Use read-only volume mounts for config files
   - Keep Docker images updated

### Known Dependencies:

- **External API:** https://newsnow.busiyi.world
- **Risk:** Service availability depends on third party
- **Mitigation:** Data cached locally, historical access preserved

---

## 📈 Project Status

**Version:** 3.0.5 (Standalone) | 1.0.1 (MCP)
**Status:** Active (Stable Fork)
**Python:** 3.10+
**License:** GPL-3.0
**Audit Score:** 7.5/10 overall, 7/10 security
**Last Audit:** 2025-11-14

**This fork:**
- ✅ Security hardened
- ✅ English documentation
- ✅ Production ready
- ⚠️ Not synced with upstream
- ⚠️ Chinese platforms focus

---

**Happy Monitoring!** 📰

For detailed Chinese documentation, see [readme.md](readme.md) (original).
