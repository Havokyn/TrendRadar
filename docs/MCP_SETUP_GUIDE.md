# ChinaNewsStream MCP Setup Guide 🤖

> **For:** AI-powered news analysis
> **Clients:** Cherry Studio, Claude Desktop, Cursor, Cline, and other MCP-compatible clients

---

## What is MCP?

**Model Context Protocol (MCP)** is an open protocol that enables AI assistants to access external tools and data sources. ChinaNewsStream provides an MCP server with 13 specialized tools for intelligent news analysis.

### What You Can Do:

- Query news using natural language: *"Show yesterday's trending topics on Zhihu"*
- Analyze trends: *"How has Bitcoin coverage changed over the last week?"*
- Find similar news: *"Find articles related to Tesla price cuts"*
- Generate reports: *"Create a weekly summary of AI news"*
- Compare platforms: *"Which platform covers tech news most actively?"*

---

## 📋 Prerequisites

- **Python 3.10+** installed
- **`uv` package manager** (auto-installed by setup scripts)
- **MCP-compatible AI client** (see options below)
- **Project files** with news data in `/output` directory

---

## 🚀 Quick Setup

### Step 1: Get the Project

**Option A: Git Clone (Recommended for tech users)**
```bash
git clone https://github.com/Havokyn/ChinaNewsStream.git
cd ChinaNewsStream
```

**Advantages:**
- Easy updates: `git pull` to get latest data
- Version control
- Sync with remote repository

**Option B: Download ZIP (For beginners)**
1. Visit: https://github.com/Havokyn/ChinaNewsStream
2. Click green "Code" button → "Download ZIP"
3. Extract to a folder (remember the path!)

**Note:** ZIP download requires manual re-download for updates

---

### Step 2: Run Setup Script

**Windows:**
```cmd
# Double-click setup-windows.bat
# OR run in Command Prompt:
setup-windows.bat
```

**macOS/Linux:**
```bash
chmod +x setup-mac.sh
./setup-mac.sh
```

**What it does:**
1. Installs `uv` package manager (if not present)
2. Creates virtual environment
3. Installs Python dependencies
4. Displays MCP configuration info

**Save the output!** You'll need:
- `uv` command path
- Project directory path
- Command-line arguments

---

## 🎯 Client Setup Guides

### Option 1: Cherry Studio (Easiest - GUI)

**1. Download Cherry Studio:**
- Website: https://cherry-ai.com/
- Windows: [Cherry-Studio-Windows.exe](https://github.com/kangfenmao/cherry-studio/releases/latest)
- macOS: [Cherry-Studio-Mac.dmg](https://github.com/kangfenmao/cherry-studio/releases/latest)

**2. Configure MCP Server:**

Open Cherry Studio:
1. Click ⚙️ **Settings** (top right)
2. Navigate to **MCP** section
3. Click **Add Server**
4. Fill in details:

```
Name: ChinaNewsStream
Description: Chinese news aggregation and analysis
Type: STDIO
Command: /path/to/uv
Arguments (one per line):
  --directory
  /path/to/ChinaNewsStream
  run
  python
  -m
  mcp_server.server
```

**Finding paths:**

**Windows - uv path:**
```cmd
where uv
```

**macOS/Linux - uv path:**
```bash
which uv
```

**Project path:** Full path to your ChinaNewsStream folder

**3. Enable and Test:**
1. Click **Save**
2. Toggle server **ON** ✅
3. Start a chat and ask: *"What tools do you have access to?"*

---

### Option 2: Claude Desktop

**1. Locate Claude Desktop config:**

**macOS:**
```bash
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```
%APPDATA%\Claude\claude_desktop_config.json
```

**2. Edit configuration:**

```json
{
  "mcpServers": {
    "chinanewsstream": {
      "command": "/path/to/uv",
      "args": [
        "--directory",
        "/path/to/ChinaNewsStream",
        "run",
        "python",
        "-m",
        "mcp_server.server"
      ]
    }
  }
}
```

**3. Restart Claude Desktop**

**4. Verify:**
Ask Claude: *"What MCP tools are available?"*

---

### Option 3: Cursor IDE

**1. Open Cursor Settings:**
- Press `Cmd+,` (Mac) or `Ctrl+,` (Windows)
- Search for "MCP"

**2. Add MCP Server:**

```json
{
  "mcp.servers": {
    "chinanewsstream": {
      "command": "/path/to/uv",
      "args": [
        "--directory",
        "/path/to/ChinaNewsStream",
        "run",
        "python",
        "-m",
        "mcp_server.server"
      ]
    }
  }
}
```

**3. Reload window** (Cmd+R / Ctrl+R)

---

### Option 4: Cline (VS Code Extension)

**1. Install Cline extension** from VS Code marketplace

**2. Configure MCP:**

Open settings → Cline → MCP Servers → Edit

```json
{
  "mcpServers": {
    "chinanewsstream": {
      "command": "/path/to/uv",
      "args": [
        "--directory",
        "/path/to/ChinaNewsStream",
        "run",
        "python",
        "-m",
        "mcp_server.server"
      ]
    }
  }
}
```

**3. Restart VS Code**

---

## 🛠️ Available Tools (13 total)

### 📊 Data Query Tools

**1. get_latest_news**
```
Get the most recent news data from all or specific platforms
Example: "Show me the latest news from Zhihu and Weibo"
```

**2. get_news_by_date**
```
Query historical news by date (supports natural language)
Example: "What was trending yesterday on Bilibili?"
```

**3. get_trending_topics**
```
Get keyword frequency statistics from your watchlist
Example: "What topics am I monitoring most frequently?"
```

---

### 🔍 Smart Search Tools

**4. search_news**
```
Unified search with multiple modes:
- Keyword: Exact match
- Fuzzy: Content similarity
- Entity: Named entity search
Example: "Search for news about Tesla using fuzzy matching"
```

**5. search_related_news_history**
```
Find related news in historical data
Example: "Find news related to 'Bitcoin price surge' from last week"
```

---

### 📈 Analytics Tools

**6. analyze_topic_trend**
```
Comprehensive trend analysis:
- Heat tracking over time
- Topic lifecycle analysis
- Viral detection (突然爆火)
- Future trend prediction
Example: "Analyze the trend of AI news over the past 7 days"
```

**7. analyze_data_insights**
```
Deep data analysis:
- Platform comparison
- Activity statistics
- Keyword co-occurrence
Example: "Compare how different platforms cover AI topics"
```

**8. analyze_sentiment**
```
Sentiment analysis of news coverage
Example: "What's the sentiment around Tesla in recent news?"
```

**9. find_similar_news**
```
Similarity detection
Example: "Find news similar to 'ChatGPT 5 release'"
```

**10. generate_summary_report**
```
Automated report generation
Example: "Generate a weekly summary report"
```

---

### ⚙️ System Tools

**11. get_current_config**
```
View current configuration
Example: "Show me the current platform configuration"
```

**12. get_system_status**
```
System health check
Example: "What's the system status?"
```

**13. trigger_crawl**
```
Manually trigger news collection
Example: "Crawl latest news from Weibo and Zhihu"
```

---

## 💡 Example Queries

### Basic Queries
```
"Show me today's trending news"
"What was popular on Zhihu yesterday?"
"Get the latest Bilibili trends"
```

### Analysis Queries
```
"Analyze how Bitcoin coverage has changed over the last week"
"Compare AI news coverage across all platforms"
"What topics have gone viral in the past 3 days?"
```

### Search Queries
```
"Find all news about Tesla price cuts"
"Search for news similar to 'iPhone 15 release'"
"Show me news containing both 'AI' and 'regulation'"
```

### Report Generation
```
"Generate a daily summary of tech news"
"Create a weekly report for cryptocurrency topics"
"Summarize this week's most important trends"
```

---

## 🔧 Troubleshooting

### Server won't start

**Check Python version:**
```bash
python --version  # Should be 3.10+
```

**Check uv installation:**
```bash
uv --version
```

**Reinstall dependencies:**
```bash
cd ChinaNewsStream
uv sync
```

---

### Client can't connect

**Verify paths are absolute:**
```
❌ Wrong: ./ChinaNewsStream
✅ Correct: /Users/username/ChinaNewsStream
```

**Check uv path:**
```bash
# macOS/Linux
which uv

# Windows
where uv
```

**Test server manually:**
```bash
cd ChinaNewsStream
uv run python -m mcp_server.server
# Should see startup messages
```

---

### No data returned

**Check if crawler has run:**
```bash
ls -la output/
# Should see dated folders with data
```

**Run crawler manually:**
```bash
python main.py
```

**Or trigger via MCP:**
```
Ask AI: "Trigger a crawl for all platforms"
```

---

### Tools not appearing in client

**Cherry Studio:**
1. Server toggle must be **ON** ✅
2. Try restarting Cherry Studio
3. Check server logs in MCP settings

**Claude Desktop:**
1. Config file must be valid JSON
2. Restart Claude Desktop completely
3. Check `~/Library/Logs/Claude/` (Mac) for errors

**Cursor/Cline:**
1. Reload window after config changes
2. Check extension logs
3. Verify MCP extension is enabled

---

## 🔄 Updating Data

### If using Git:
```bash
cd ChinaNewsStream
git pull  # Get latest data from remote
```

### If using ZIP download:
1. Download new ZIP
2. Extract to same location
3. **Keep your config files** (don't overwrite)
4. Copy `output/` folder from old to new

---

## 📚 Advanced Configuration

### HTTP Mode (For remote access)

**Start server in HTTP mode:**
```bash
uv run python -m mcp_server.server --transport http --port 3333
```

**Client configuration:**
```json
{
  "mcpServers": {
    "chinanewsstream": {
      "url": "http://localhost:3333/mcp"
    }
  }
}
```

---

### Custom Project Root

**If data is in different location:**
```bash
uv run python -m mcp_server.server --project-root /path/to/data
```

---

## 🆘 Getting Help

**Common Issues:**
- **"Module not found"**: Run `uv sync` again
- **"Permission denied"**: Use `chmod +x` on scripts
- **"Connection refused"**: Check firewall settings

**Resources:**
- Project Issues: https://github.com/Havokyn/ChinaNewsStream/issues
- MCP Protocol Docs: https://modelcontextprotocol.io/
- Original Project: https://github.com/sansan0/TrendRadar

---

## 🎉 Success Checklist

- [ ] Setup script ran successfully
- [ ] MCP server starts without errors
- [ ] Client shows 13 available tools
- [ ] Test query returns data
- [ ] Can trigger manual crawl

**You're ready to start analyzing!** 🚀

---

## 📝 Notes

**Data Sources:**
- All news data from https://newsnow.busiyi.world API
- Local caching for performance
- Historical data stored in `/output`

**Performance:**
- First query may be slow (cache building)
- Subsequent queries are fast (< 1 second)
- 15-minute cache TTL for most queries

**Privacy:**
- All analysis runs locally
- No data sent to external servers (except original news API)
- MCP communication stays on your machine

---

**Happy Analyzing!** 📊
