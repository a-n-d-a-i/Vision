# NanoClaw

**A minimal autonomous AI agent in a single file.**

Telegram → Claude Code bridge with persistent sessions, heartbeat monitoring, and cron scheduling.

## Features

- 💬 **Chat Interface** - Control Claude Code via Telegram
- 🧠 **Persistent Sessions** - Context maintained across conversations
- 💓 **Heartbeat Monitoring** - Autonomous background checks every 5 minutes
- ⏰ **Cron Scheduling** - Time-based task automation
- 🔔 **Proactive Alerts** - Agent can notify you when conditions are met
- 🛠️ **Full Agent Capabilities** - Bash, file operations, web search, etc.

## Why NanoClaw?

OpenClaw has 100,000+ lines of code. NanoClaw is ~400 lines and does the same core job:
- ✅ Telegram interface
- ✅ Persistent memory
- ✅ Autonomous monitoring
- ✅ Scheduled tasks
- ✅ Tool use (bash, files, web)
- ❌ No OAuth complexity
- ❌ No skill marketplace bloat
- ❌ No security nightmare

**1000x less code. 100% of the value.**

## Setup

### Prerequisites

1. **Anthropic API Key** - Get from [console.anthropic.com](https://console.anthropic.com)
2. **Telegram Bot** - Create via [@BotFather](https://t.me/botfather)
3. **Your Telegram Chat ID** - Get from [@userinfobot](https://t.me/userinfobot)

### Installation

```bash
# Install dependencies
npm install

# Set environment variables
export ANTHROPIC_API_KEY="sk-ant-..."
export TELEGRAM_BOT_TOKEN="123456:ABC-DEF..."
export MY_TG_CHAT_ID="123456789"

# Run
npm start
```

### Environment Variables

Create a `.env` file:

```bash
ANTHROPIC_API_KEY=sk-ant-your-key-here
TELEGRAM_BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
MY_TG_CHAT_ID=123456789
```

Then use `dotenv` or export them before running.

## Usage

### Basic Chat

Just message your bot on Telegram:

```
You: What files are in /agent?
JARVIS: [uses Bash tool to list files]

You: Create a Python script that analyzes them
JARVIS: [creates script, runs it, shows results]
```

### Commands

- `/start` - Initialize bot
- `/reset` - Clear current session
- `/status` - Show system status
- `/heartbeat` - Force heartbeat check now
- `/cron` - List scheduled tasks

### Heartbeat Monitoring

Edit `HEARTBEAT.md` to configure autonomous monitoring:

```markdown
# Heartbeat Monitoring

## Always Check
- [ ] Monitor disk usage - alert if >85%
- [ ] Check for failed services
- [ ] Scan logs for errors

## Scheduled Tasks
CRON[0 8 * * *]: Send morning briefing
CRON[0 18 * * 1-5]: End of day summary
```

The agent will:
1. Check these conditions every 5 minutes
2. Execute any necessary actions
3. Write alerts to `ALERTS.txt`
4. Send alerts to your Telegram

### Cron Jobs

Add cron directives to `HEARTBEAT.md`:

```markdown
CRON[0 8 * * *]: Check calendar and send daily briefing
CRON[*/30 * * * *]: Monitor server health, restart if needed
CRON[0 12 * * 0]: Weekly summary of accomplishments
```

Standard cron syntax:
```
* * * * *
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, 0 and 7 are Sunday)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

### Alerts

The agent sends alerts by writing to `ALERTS.txt`:

```bash
# In bash tool:
echo "🔔 Server load is high: 8.5" >> ALERTS.txt
```

The alert monitor checks every 30 seconds and sends to Telegram.

## File Structure

```
nanoclaw/
├── nanoclaw.ts           # Main file (~400 lines)
├── package.json          # Dependencies
├── HEARTBEAT.md          # Monitoring checklist
├── history.jsonl         # Chat history
├── heartbeat-state.json  # Heartbeat state
├── cron-state.json       # Cron jobs
└── ALERTS.txt            # Pending alerts
```

## Configuration

Edit these constants in `nanoclaw.ts`:

```typescript
const ASSISTANT_NAME = 'JARVIS';           // Bot name
const HEARTBEAT_INTERVAL = 5 * 60 * 1000;  // 5 minutes
const ALERT_CHECK_INTERVAL = 30 * 1000;    // 30 seconds
const MAX_HISTORY = 100;                   // Message history limit
```

## Examples

### Server Monitoring

```markdown
# HEARTBEAT.md

## Always Check
- [ ] Check if nginx is running - restart if down
- [ ] Monitor disk usage - clean logs if >90%
- [ ] Scan /var/log/app.log for ERROR - investigate and fix
```

### Daily Automation

```markdown
CRON[0 8 * * *]: Morning routine:
1. Check weather for today
2. Review calendar appointments
3. List top 3 priorities from TODO.md
4. Send briefing via ALERTS.txt

CRON[0 18 * * 1-5]: Evening summary:
1. What got accomplished today
2. Any blockers or issues
3. Tomorrow's priorities
4. Save to memory/daily-logs/YYYY-MM-DD.md
```

### Proactive Assistance

```markdown
## Event-Driven
- [ ] If new email from boss - summarize immediately
- [ ] If GitHub Actions fails - investigate and fix
- [ ] If crypto portfolio drops >10% - alert with analysis
- [ ] If meeting in 15 mins - send reminder with prep notes
```

## How It Works

### Architecture

```
Telegram <-> NanoClaw <-> Claude Code <-> /agent filesystem
                │
                ├─> Heartbeat (every 5 min)
                ├─> Cron Jobs (scheduled)
                └─> Alert Monitor (every 30s)
```

### Session Management

- Each Telegram chat gets a persistent Claude Code session
- Sessions survive restarts (stored in `history.jsonl`)
- Use `/reset` to start fresh

### Heartbeat System

1. Every 5 minutes, reads `HEARTBEAT.md`
2. Passes checklist to Claude Code
3. Claude evaluates each condition
4. Takes actions if needed
5. Writes alerts to `ALERTS.txt`
6. Updates state in `heartbeat-state.json`

### Alert Flow

1. Agent writes to `ALERTS.txt`: `echo "message" >> ALERTS.txt`
2. Alert monitor checks file every 30s
3. If content found, sends to Telegram
4. Clears file after sending

## Comparison to OpenClaw

| Feature | OpenClaw | NanoClaw |
|---------|----------|----------|
| Lines of Code | ~100,000 | ~400 |
| Setup Time | 2-4 hours | 2 minutes |
| OAuth Required | Yes | No |
| Multi-Platform | WhatsApp, Discord, Slack, Signal | Telegram |
| Heartbeat | ✅ | ✅ |
| Cron Jobs | ✅ | ✅ |
| Persistent Memory | ✅ | ✅ |
| Tool Use | ✅ | ✅ |
| Skills Marketplace | ✅ | ❌ |
| Security Concerns | High | Low |
| Maintenance | Complex | Trivial |
| API Cost | $10-150/month | Same |
| Extensibility | Plugin system | Edit 1 file |

## Advanced Usage

### Custom Tools

Claude Code supports these tools by default:
- `Bash` - Run shell commands
- `Read` - Read files
- `Edit` - Edit files with search/replace
- `Write` - Create new files
- `Glob` - Find files by pattern
- `Grep` - Search file contents
- `Task` - Delegate sub-tasks
- `WebFetch` - Fetch URLs
- `WebSearch` - Search the web
- `Skill` - Use installed skills

### Working Directory

The agent operates in `/agent` by default. You can:

```bash
# Mount your project directory
docker run -v /path/to/project:/agent ...

# Or change cwd in code:
cwd: '/home/user/projects'
```

### Multiple Chats

Add more Telegram chat IDs:

```typescript
const ALLOWED_CHAT_IDS = [
  123456789,  // Your personal chat
  987654321,  // Team chat
];
```

Each chat gets its own session.

## Troubleshooting

### Bot not responding

1. Check `TELEGRAM_BOT_TOKEN` is correct
2. Verify chat ID is in `ALLOWED_CHAT_IDS`
3. Check Anthropic API key has credits

### Heartbeat not running

1. Verify `HEARTBEAT.md` exists
2. Check console for errors
3. Use `/heartbeat` to force run

### Alerts not sending

1. Check `ALERTS.txt` is being written
2. Verify bot has Telegram access
3. Check console for alert monitor errors

## Security

NanoClaw is simpler and more secure than OpenClaw:

- ✅ No exposed ports (no gateway to hack)
- ✅ No OAuth complexity (just API keys)
- ✅ Single chat ID whitelist
- ✅ All code in one file (easy to audit)
- ⚠️ Agent has full bash access (only allow trusted users)
- ⚠️ Anthropic API key stored in env (use secrets manager in prod)

## Cost

Same as OpenClaw:
- $0 for the code (open source)
- ~$10-150/month for Anthropic API usage
  - Use Claude Haiku 4.5 for cheaper operation
  - Use Claude Opus 4.5 for max capability

## Development

Run in watch mode:

```bash
npm run dev
```

Edit `nanoclaw.ts` and it auto-reloads.

## Future Ideas

Possible additions (each ~50 lines):

- [ ] Multi-user support with per-user permissions
- [ ] Voice message transcription
- [ ] Image analysis via vision API
- [ ] Web UI for managing heartbeat/cron
- [ ] Docker compose for easy deployment
- [ ] Metrics dashboard
- [ ] Integration with n8n/Zapier webhooks
- [ ] Notion/Obsidian sync

But the point is to stay minimal. Fork and customize as needed.

## Philosophy

> "Perfection is achieved not when there is nothing more to add, but when there is nothing left to take away." - Antoine de Saint-Exupéry

NanoClaw proves you don't need 100,000 lines to build a powerful autonomous AI agent.

OpenClaw is impressive, but it's solving enterprise problems most people don't have. NanoClaw focuses on the 20% of features that deliver 80% of the value.

## License

MIT - Do whatever you want with it

## Credits

- Inspired by OpenClaw by Peter Steinberger
- Built on Claude Code by Anthropic
- Telegram interface via Grammy

## Contributing

This is intentionally a single-file project. If you want to add features:

1. Fork it
2. Keep it minimal
3. Share your version

The goal is simplicity, not feature completeness.

---

**Built by someone tired of bloated codebases.**

**400 lines. Full autonomy. Zero bullshit.**
