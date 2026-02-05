# Heartbeat Monitoring Checklist

This file defines what ${ASSISTANT_NAME} monitors autonomously.
The heartbeat runs every 5 minutes and evaluates each item.

## Always Check (Every Heartbeat)

- [ ] Monitor disk usage - alert if any partition >85%
- [ ] Check system load average - alert if >8.0
- [ ] Scan for failed systemd services - alert if any critical services down
- [ ] Check memory usage - alert if <10% free
- [ ] Monitor Docker containers - alert if any unexpected stops

## Time-Based Checks

Use CRON syntax for scheduled tasks:

CRON[0 8 * * *]: Send morning briefing (weather, calendar summary, top 3 priorities for today)
CRON[0 18 * * 1-5]: End of workday summary - what got done today, save to memory/daily-logs/
CRON[0 12 * * 0]: Weekly review - analyze past week's accomplishments, insights, patterns
CRON[*/30 * * * *]: Check email for urgent messages from important contacts

## Event-Driven Monitoring

- [ ] If new file appears in ~/Downloads - analyze and auto-organize to appropriate folder
- [ ] If error appears in /var/log/app.log - investigate and attempt fix
- [ ] If GitHub Actions workflow fails - read logs and open PR with fix
- [ ] If cryptocurrency portfolio drops >10% - immediate alert with analysis
- [ ] If calendar shows meeting in next 15 minutes - send reminder with meeting prep

## Proactive Assistance

- [ ] Check if TODO.md has tasks marked urgent - nudge me if overdue
- [ ] Monitor weather - alert if conditions change significantly
- [ ] Track personal metrics (if integrated) - notify of interesting patterns
- [ ] Watch for system updates - apply non-breaking updates automatically

## Intelligence Guidelines

When evaluating conditions:
- Don't spam - only alert when genuinely important
- Learn from my responses - if I ignore certain alerts, adjust thresholds
- Be context-aware - don't interrupt during deep work blocks
- Proactively solve problems - try fixes before alerting
- Document patterns - track what triggers alerts and outcomes

## Status & Memory

Last successful heartbeat: [auto-updated]
Recent alerts sent: [auto-tracked]
System health: [auto-updated]

## Alert Examples

Good alert (actionable):
```
🔔 Disk usage at 87% on /dev/sda1
Largest directories:
- /var/log/old-logs (23GB)
- ~/.cache/builds (18GB)
Should I clean these up?
```

Bad alert (noisy):
```
Disk usage is 76%
```

## Notes for ${ASSISTANT_NAME}

- You have full bash access - use it to investigate before alerting
- Write alerts to ALERTS.txt using: echo "message" >> ALERTS.txt
- Update this file with status/findings
- Learn and adapt - if a check becomes irrelevant, mark it as such
- You can add new checks as you discover patterns in my work
