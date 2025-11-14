# ChinaNewsStream

A fork of [TrendRadar](https://github.com/sansan0/TrendRadar) - News trend aggregation tool for Chinese platforms.

**Original Project:** [sansan0/TrendRadar](https://github.com/sansan0/TrendRadar)
**License:** GPL-3.0
**Fork Purpose:** Personal deployment with stable configuration

---

## What This Fork Does

Aggregates trending news from 11+ major Chinese platforms:
- Zhihu (知乎)
- Weibo (微博)
- Douyin (抖音)
- Bilibili
- Baidu (百度)
- Toutiao (今日头条)
- And more...

Filters news by custom keywords and sends notifications to your preferred channels.

---

## Quick Start (Docker - Recommended)

### 1. Clone This Repository

```bash
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream
```

### 2. Configure

```bash
# Copy environment template
cp .env.example .env

# Edit with your webhook URLs and settings
nano .env  # or use your preferred editor
```

### 3. Set Up Keywords

Edit `config/frequency_words.txt` to add topics you want to monitor:

```text
# Add one keyword per line or in groups
人工智能
AI
特斯拉
比亚迪
```

### 4. Run with Docker

```bash
cd docker
docker-compose up -d
```

### 5. Check Logs

```bash
docker logs -f trend-radar
```

---

## Configuration Priority

Settings are loaded in this order (later overrides earlier):

1. `config/config.yaml` (base configuration)
2. Environment variables in `.env` (overrides YAML)
3. Docker environment variables (highest priority)

**Recommendation:** Use `.env` for secrets, `config.yaml` for platforms/weights.

---

## Security Setup (IMPORTANT)

### ✅ DO:
- Copy `.env.example` to `.env` and fill in secrets
- Store webhook URLs in `.env` (NOT in `config/config.yaml`)
- Keep `.env` and `config/config.yaml` out of version control
- Use strong passwords for email authentication

### ❌ DON'T:
- Commit `config/config.yaml` with real webhooks
- Share `.env` file publicly
- Push `output/` directory to GitHub

---

## Notification Channels

Configure any or all of these in `.env`:

| Channel | Required Variables |
|---------|-------------------|
| **Feishu/Lark** | `FEISHU_WEBHOOK_URL` |
| **DingTalk** | `DINGTALK_WEBHOOK_URL` |
| **WeChat Work** | `WEWORK_WEBHOOK_URL` |
| **Telegram** | `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID` |
| **Email** | `EMAIL_FROM`, `EMAIL_PASSWORD`, `EMAIL_TO` |
| **ntfy.sh** | `NTFY_TOPIC` |

---

## Deployment Options

### Option 1: Docker (Recommended)
- Easiest setup
- Automated scheduling
- Isolated environment

```bash
cd docker
docker-compose up -d
```

### Option 2: GitHub Actions
- Free hosting
- Runs on schedule
- See original repo for workflow setup

### Option 3: Local Python
- Full control
- Manual execution

```bash
# macOS/Linux
./setup-mac.sh
python main.py

# Windows
setup-windows.bat
python main.py
```

### Option 4: MCP Server (AI Integration)
- Use with Claude Desktop, Cherry Studio, etc.

```bash
uv run python -m mcp_server.server
```

---

## File Structure

```
ChinaNewsStream/
├── config/
│   ├── config.yaml          # Platform & weight settings
│   └── frequency_words.txt  # Keywords to monitor
├── output/                  # Generated reports (auto-created)
├── docker/                  # Docker deployment files
├── mcp_server/              # MCP server implementation
├── main.py                  # Standalone crawler script
├── .env                     # Your secrets (DO NOT COMMIT)
└── .env.example             # Template for .env
```

---

## Customization

### Change Monitored Platforms

Edit `config/config.yaml`:

```yaml
platforms:
  - id: "zhihu"
    name: "知乎"
  - id: "weibo"
    name: "微博"
  # Add more from https://newsnow.busiyi.world/
```

### Adjust Push Mode

In `.env`:

```bash
REPORT_MODE=daily        # Daily summary
REPORT_MODE=incremental  # Only new items
REPORT_MODE=current      # Current trending
```

### Set Push Time Window

```bash
PUSH_WINDOW_ENABLED=true
PUSH_WINDOW_START=09:00
PUSH_WINDOW_END=18:00
PUSH_WINDOW_ONCE_PER_DAY=true
```

---

## Troubleshooting

### No Notifications Received
1. Check `.env` has correct webhook URLs
2. Verify `ENABLE_NOTIFICATION=true`
3. Check Docker logs: `docker logs trend-radar`
4. Test webhook URLs manually

### No Data in Output
1. Verify `ENABLE_CRAWLER=true`
2. Check network connectivity to `https://newsnow.busiyi.world`
3. Check if proxy settings are correct (if using)

### Permission Errors (Docker)
```bash
# Fix output directory permissions
chmod -R 777 output/
```

---

## Differences from Original

This fork:
- ✅ Includes `.gitignore` for security
- ✅ Provides `.env.example` template
- ✅ Adds this setup documentation
- ✅ Configured for stable deployment
- ⚠️ **Will not be kept in sync** with upstream

If you need latest features, use the [original repository](https://github.com/sansan0/TrendRadar).

---

## License & Attribution

**License:** GNU General Public License v3.0

This project is a fork of [TrendRadar](https://github.com/sansan0/TrendRadar) by sansan0.

Original project uses data from [newsnow](https://github.com/ourongxing/newsnow).

Under GPL-3.0, you are free to:
- ✅ Use for any purpose
- ✅ Study and modify the code
- ✅ Distribute copies
- ⚠️ **Must keep derivative works open source**
- ⚠️ **Must maintain copyright notices**

---

## Support & Resources

- **Original Project:** https://github.com/sansan0/TrendRadar
- **Original Docs:** See `readme.md` in root directory
- **Data Source:** https://newsnow.busiyi.world/
- **Issues:** Open an issue in this repo for fork-specific problems

---

## Audit Summary

This fork was audited on 2025-11-14:

**Security:** 7/10 - Good with proper credential management
**Code Quality:** 7/10 - Functional, well-documented
**Build Ready:** ✅ YES

**Critical Items Addressed:**
- ✅ Added `.gitignore`
- ✅ Added `.env.example`
- ✅ Documented security best practices

**Known Limitations:**
- Depends on external API (`newsnow.busiyi.world`)
- Large monolithic `main.py` (works but could be refactored)
- No automated tests

See full audit report in commit history.

---

## Quick Reference

### Start Docker
```bash
cd docker && docker-compose up -d
```

### Stop Docker
```bash
cd docker && docker-compose down
```

### View Logs
```bash
docker logs -f trend-radar
```

### Manual Run
```bash
python main.py
```

### Update Configuration
1. Edit `.env` or `config/config.yaml`
2. Restart: `docker-compose restart`

---

**Happy Monitoring!** 📰
