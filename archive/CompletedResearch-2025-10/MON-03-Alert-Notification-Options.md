# MON-03: Alert Notification Options

**Research Date:** October 11, 2025
**Status:** Complete
**Confidence Level:** High
**Researcher:** Claude Code (Sonnet 4.5)

---

## Executive Summary

Based on comprehensive research of notification methods for home server monitoring, **ntfy (self-hosted) as primary with Telegram as backup** is the recommended solution. This combination provides:

- **Zero ongoing costs** (both are free)
- **Excellent reliability** (two independent channels)
- **Easy Docker integration** (ntfy runs alongside other services)
- **Low resource usage** (~128MB RAM for ntfy)
- **Script compatibility** (both work from shell scripts and Grafana)
- **ARM/Le Potato optimized** (proven on Raspberry Pi ARM systems)

---

## 1. Notification Methods Evaluation

### 1.1 Email (via Gmail SMTP)

**Pros:**
- Universal - everyone has email
- Built-in to Grafana alerting
- Free for personal use
- Good for non-urgent alerts and summaries
- Reliable delivery infrastructure

**Cons:**
- Gmail SMTP requires app-specific passwords (security complexity)
- Delayed delivery (not real-time)
- Can get lost in inbox noise
- May be filtered to spam for automated messages
- Not ideal for critical alerts requiring immediate attention

**Verdict:** ✅ Good for **WARNING** alerts, not for CRITICAL

---

### 1.2 Discord Webhook

**Pros:**
- Free with no limits on webhooks
- Native Grafana integration (built-in contact point)
- Real-time notifications
- Can organize by channels
- Mobile and desktop apps available
- Rich formatting support (embeds, markdown)

**Cons:**
- Requires Discord account and keeping app open
- Can be noisy if used for other purposes
- No offline notification queuing
- Alert history tied to Discord message retention

**Setup Difficulty:** Easy (create webhook URL, paste into Grafana)

**Verdict:** ✅ Good option, but consider alert fatigue if Discord is used socially

---

### 1.3 Telegram Bot

**Pros:**
- Free with no limits
- Native Grafana integration
- Excellent mobile app with reliable push notifications
- Can create dedicated bot for monitoring
- Message limit: 4096 characters (sufficient for alerts)
- Lightweight mobile app
- Works great for personal/small team use

**Cons:**
- Requires Telegram account
- Initial setup with BotFather
- Less popular in some regions than other messengers

**Setup Process:**
1. Message @BotFather on Telegram
2. Create new bot with `/newbot`
3. Receive API token
4. Get your chat ID (message bot, use API to retrieve)
5. Configure in Grafana or scripts

**Resource Usage:** Negligible (API-based, no self-hosting)

**Verdict:** ✅ **HIGHLY RECOMMENDED** - Excellent balance of features and simplicity

---

### 1.4 ntfy.sh (Self-Hosted)

**Pros:**
- 100% free and open source
- Self-hosted = full control and privacy
- Extremely lightweight (~128MB RAM, 500m CPU)
- Perfect for ARM/Raspberry Pi (proven compatibility)
- No database required
- Docker deployment in <10 minutes
- Works with shell scripts via simple PUT/POST requests
- Push notifications to mobile via ntfy app
- Can be integrated with Grafana via webhook
- Web interface for viewing notification history

**Cons:**
- Requires hosting (but on your existing server)
- One more service to maintain
- Mobile app less polished than commercial services
- Need to expose service or use VPN for external notifications

**Setup Process:**
```yaml
# docker-compose.yml
services:
  ntfy:
    image: binwiederhier/ntfy
    container_name: ntfy
    command: serve
    ports:
      - "8080:80"
    volumes:
      - /var/cache/ntfy:/var/cache/ntfy
    restart: unless-stopped
    environment:
      - NTFY_BASE_URL=http://your-server:8080
```

**Shell Script Usage:**
```bash
# Send notification
curl -d "Pi-hole is down!" ntfy.sh/homeserver-alerts
# Or self-hosted:
curl -d "Pi-hole is down!" http://localhost:8080/homeserver-alerts
```

**Resource Usage on Le Potato:**
- Memory: ~50-128MB
- CPU: Minimal (only when sending notifications)
- Storage: ~50MB for binary + cache

**ARM Compatibility:** ✅ Excellent - Docker images available for armv6, armv7, and arm64

**Verdict:** ✅ **HIGHLY RECOMMENDED** - Best for privacy, control, and homelab integration

---

### 1.5 Pushover

**Pros:**
- Professional, polished mobile apps
- Extremely reliable delivery
- One-time payment model ($4.99 per platform)
- Priority notification support
- 10,000 messages/month free tier
- Native Grafana integration
- Works well from shell scripts

**Cons:**
- Not free (though one-time payment is reasonable)
- Requires account creation
- Closed source / proprietary
- $4.99 per platform (iOS, Android, Desktop separate)

**Cost Analysis:**
- Personal use: $4.99 one-time (iOS or Android)
- Business: $5/user/month
- API calls: Free up to 10,000/month

**Verdict:** ✅ Good option if budget allows, but ntfy + Telegram are free alternatives

---

### 1.6 SMS (Twilio, AWS SNS)

**Pros:**
- Works without internet connectivity
- Universal - no app required
- Highest reliability for critical alerts

**Cons:**
- **Ongoing costs:** $0.0075-$0.02 per SMS (Twilio)
- Can get expensive with frequent alerts
- Limited formatting
- No alert history/dashboard
- Requires phone number management

**Cost Estimate (monthly):**
- 100 alerts/month: ~$1-2
- 500 alerts/month: ~$5-10
- 1000 alerts/month: ~$10-20

**Verdict:** ❌ Not recommended - Cost prohibitive for homelab use, better for commercial systems

---

### 1.7 Grafana Built-in Alerting

**Evaluation:**
Grafana alerting is not a notification *method* but rather the **alert engine** that sends to notification channels. It's highly recommended to use Grafana as the alerting platform with one or more of the above notification channels.

**Grafana Alerting Features:**
- Contact Points (notification destinations)
- Notification Policies (routing and grouping)
- Alert Rules (conditions and evaluation)
- Silences and Mute Timings
- Test notifications before deployment
- Template support for custom messages
- Deduplication and grouping

**Supported Integrations:**
- Email, Discord, Telegram, Slack, webhook (ntfy), Pushover, PagerDuty, OpsGenie, and 20+ more

**Verdict:** ✅ **MANDATORY** - Use Grafana as the alerting engine, send to ntfy/Telegram

---

## 2. Additional Tools Considered

### 2.1 Uptime Kuma

**What it is:** A self-hosted uptime monitoring tool (like Pingdom/UptimeRobot)

**Strengths:**
- Beautiful, user-friendly interface
- Built-in alerting to 90+ notification services
- HTTP/HTTPS, TCP, ping, DNS, Docker container monitoring
- Status pages
- Very lightweight and Docker-friendly
- Certificate expiry monitoring

**Use Case:** Service availability monitoring (HTTP endpoints, TCP ports)

**Recommendation:** ✅ Consider as **complementary tool** to Grafana
- Use Uptime Kuma for: Service uptime checks (Pi-hole web UI, Tailscale status)
- Use Grafana for: System metrics (CPU, RAM, disk, temperature)

**Integration Strategy:**
- Run Uptime Kuma alongside Grafana
- Configure Uptime Kuma to send alerts to ntfy/Telegram
- Provides redundancy (if Grafana goes down, Uptime Kuma still monitors)

---

## 3. Recommended Configuration

### Primary Architecture: Grafana + ntfy + Telegram

```
┌─────────────────────────────────────────────────────┐
│                   Le Potato Server                   │
│                                                      │
│  ┌──────────────┐                                   │
│  │   Grafana    │─────┐                            │
│  │   Alerting   │     │                            │
│  └──────────────┘     │                            │
│                       ├───► ntfy (self-hosted)     │
│  ┌──────────────┐     │       │                    │
│  │ Uptime Kuma  │─────┘       └──► ntfy app       │
│  │  (optional)  │                   (mobile)       │
│  └──────────────┘                                   │
│                       └───► Telegram Bot ──► Phone │
│                                                      │
│  ┌──────────────┐                                   │
│  │ Shell Scripts│───► curl to ntfy or Telegram     │
│  └──────────────┘                                   │
└─────────────────────────────────────────────────────┘
```

### Alert Routing Strategy

**CRITICAL Alerts → Both ntfy AND Telegram**
- Service down (Pi-hole, Tailscale, Docker)
- Disk full (>90%)
- Temperature >58°C
- SD card I/O errors
- SSD disconnection

**WARNING Alerts → ntfy only**
- Container restarts (>3 in 10 minutes)
- High log volume
- Disk space 75-90%
- Temperature 55-58°C

**INFO Alerts → Email daily digest**
- Daily system health summary
- Backup completion status
- Weekly uptime report

---

## 4. Setup Instructions

### 4.1 Deploy ntfy via Docker Compose

Create `/opt/docker/ntfy/docker-compose.yml`:

```yaml
version: "3"
services:
  ntfy:
    image: binwiederhier/ntfy
    container_name: ntfy
    command:
      - serve
    environment:
      - TZ=America/Los_Angeles  # Adjust to your timezone
      - NTFY_BASE_URL=http://lepotato.local:8080
      - NTFY_CACHE_FILE=/var/cache/ntfy/cache.db
      - NTFY_AUTH_FILE=/var/cache/ntfy/auth.db
      - NTFY_AUTH_DEFAULT_ACCESS=deny-all
    volumes:
      - ./cache:/var/cache/ntfy
      - ./config:/etc/ntfy
    ports:
      - "8080:80"
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "-q", "--tries=1", "--spider", "http://localhost/v1/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s
```

**Deploy:**
```bash
cd /opt/docker/ntfy
docker-compose up -d
```

**Secure ntfy (recommended):**
```bash
# Create user for publishing alerts
docker exec -it ntfy ntfy user add --role=admin alerts
# Enter password when prompted

# Now alerts require authentication
```

**Test:**
```bash
# Public test:
curl -d "Test alert from Le Potato" http://lepotato.local:8080/homeserver

# Authenticated test:
curl -u alerts:password -d "Authenticated test" http://lepotato.local:8080/homeserver
```

### 4.2 Configure Telegram Bot

**Step 1: Create bot**
```
1. Open Telegram, search for @BotFather
2. Send: /newbot
3. Name: Le Potato Alerts
4. Username: lepotato_alerts_bot (must end in _bot)
5. Save the API token (looks like 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw)
```

**Step 2: Get Chat ID**
```
1. Message your new bot: /start
2. Visit in browser: https://api.telegram.org/bot<TOKEN>/getUpdates
3. Look for "chat":{"id":123456789}
4. Save this chat ID
```

**Step 3: Test**
```bash
curl -X POST "https://api.telegram.org/bot<TOKEN>/sendMessage" \
  -d "chat_id=<CHAT_ID>" \
  -d "text=Test from Le Potato"
```

### 4.3 Configure Grafana Contact Points

**Navigate to:** Alerts & IRM → Alerting → Contact Points

**Add ntfy Contact Point:**
```
Name: ntfy-homeserver
Integration: Webhook
URL: http://ntfy:80/homeserver-critical
HTTP Method: POST
Authorization: Basic (if you set up auth)
  Username: alerts
  Password: [your password]

Title: {{ .GroupLabels.alertname }}
Message: {{ .CommonAnnotations.summary }}
```

**Add Telegram Contact Point:**
```
Name: telegram-critical
Integration: Telegram
Bot Token: [your bot token from BotFather]
Chat ID: [your chat ID from getUpdates]
Message:
🚨 {{ .GroupLabels.alertname }}

{{ .CommonAnnotations.summary }}

{{ .CommonAnnotations.description }}

Time: {{ .StartsAt.Format "2006-01-02 15:04:05" }}
```

**Test Both Contact Points:**
- Click "Test" button next to each contact point
- Verify you receive notifications
- If test fails, check logs: `docker logs grafana`

### 4.4 Create Notification Policies

**Navigate to:** Alerts & IRM → Alerting → Notification Policies

**Critical Alert Policy:**
```
Matching labels: severity=critical
Contact points:
  - ntfy-homeserver
  - telegram-critical
Group by: alertname
Group wait: 30s
Group interval: 5m
Repeat interval: 4h
```

**Warning Alert Policy:**
```
Matching labels: severity=warning
Contact points: ntfy-homeserver
Group by: alertname
Group wait: 1m
Group interval: 10m
Repeat interval: 12h
```

**Default Policy:**
```
Contact points: ntfy-homeserver
Group wait: 2m
Group interval: 15m
Repeat interval: 24h
```

---

## 5. Sample Alert Rules

### 5.1 Critical: Service Down (Pi-hole)

```yaml
Alert name: PiHole Service Down
Folder: HomeServer / Services
Evaluation interval: 1m
For: 2m

Query A:
  Data source: Prometheus
  Metric: up{job="pihole"}

Condition:
  WHEN last() OF A IS BELOW 1

Annotations:
  summary: Pi-hole service is not responding
  description: Pi-hole has been down for more than 2 minutes. DNS resolution may be impacted.

Labels:
  severity: critical
  service: pihole
```

### 5.2 Critical: Disk Space

```yaml
Alert name: Disk Almost Full
Folder: HomeServer / Storage
Evaluation interval: 5m
For: 5m

Query A:
  Data source: Prometheus
  Metric: (node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"}) * 100

Condition:
  WHEN last() OF A IS BELOW 10

Annotations:
  summary: Disk space critically low
  description: Root filesystem is {{ printf "%.1f" $values.A.Value }}% full. Immediate action required.

Labels:
  severity: critical
  category: storage
```

### 5.3 Critical: Temperature

```yaml
Alert name: CPU Temperature High
Folder: HomeServer / Hardware
Evaluation interval: 1m
For: 3m

Query A:
  Data source: Prometheus
  Metric: node_hwmon_temp_celsius{chip="cpu_thermal"}

Condition:
  WHEN last() OF A IS ABOVE 58

Annotations:
  summary: CPU temperature exceeds safe threshold
  description: CPU temperature is {{ printf "%.1f" $values.A.Value }}°C (threshold: 58°C). Check cooling.

Labels:
  severity: critical
  category: thermal
```

### 5.4 Warning: Container Restarts

```yaml
Alert name: Container Restarting Frequently
Folder: HomeServer / Docker
Evaluation interval: 5m
For: 10m

Query A:
  Data source: Prometheus
  Metric: rate(docker_container_restarts[10m])

Condition:
  WHEN last() OF A IS ABOVE 0.3  # More than 3 restarts in 10 minutes

Annotations:
  summary: Container {{ $labels.name }} is restarting frequently
  description: Container has restarted {{ printf "%.0f" $values.A.Value }} times in the last 10 minutes.

Labels:
  severity: warning
  service: docker
```

### 5.5 Warning: Temperature Trending High

```yaml
Alert name: CPU Temperature Warning
Folder: HomeServer / Hardware
Evaluation interval: 1m
For: 5m

Query A:
  Data source: Prometheus
  Metric: node_hwmon_temp_celsius{chip="cpu_thermal"}

Condition:
  WHEN last() OF A IS ABOVE 55
  AND last() OF A IS BELOW 58

Annotations:
  summary: CPU temperature elevated
  description: CPU temperature is {{ printf "%.1f" $values.A.Value }}°C (warning threshold: 55°C).

Labels:
  severity: warning
  category: thermal
```

---

## 6. Shell Script Integration

### 6.1 Send to ntfy from Scripts

**Basic notification:**
```bash
#!/bin/bash
# /usr/local/bin/notify-ntfy.sh

NTFY_URL="http://localhost:8080/homeserver-critical"
MESSAGE="$1"
PRIORITY="${2:-default}"  # default, low, high, urgent

curl -H "Priority: $PRIORITY" \
     -H "Tags: warning" \
     -d "$MESSAGE" \
     "$NTFY_URL"
```

**Usage in monitoring script:**
```bash
#!/bin/bash
# Check if Pi-hole is running

if ! docker ps | grep -q pihole; then
    /usr/local/bin/notify-ntfy.sh \
        "CRITICAL: Pi-hole container is not running!" \
        "urgent"
fi
```

### 6.2 Send to Telegram from Scripts

```bash
#!/bin/bash
# /usr/local/bin/notify-telegram.sh

BOT_TOKEN="your_bot_token_here"
CHAT_ID="your_chat_id_here"
MESSAGE="$1"

curl -s -X POST "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
     -d "chat_id=${CHAT_ID}" \
     -d "text=${MESSAGE}" \
     -d "parse_mode=HTML" \
     > /dev/null
```

### 6.3 Dual Notification Function

```bash
#!/bin/bash
# /usr/local/bin/notify-critical.sh
# Send to BOTH ntfy and Telegram for critical alerts

NTFY_URL="http://localhost:8080/homeserver-critical"
BOT_TOKEN="your_bot_token_here"
CHAT_ID="your_chat_id_here"
MESSAGE="$1"

# Send to ntfy
curl -H "Priority: urgent" \
     -H "Tags: rotating_light" \
     -d "$MESSAGE" \
     "$NTFY_URL" &

# Send to Telegram
curl -s -X POST "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
     -d "chat_id=${CHAT_ID}" \
     -d "text=🚨 CRITICAL: ${MESSAGE}" \
     > /dev/null &

wait
echo "Alert sent to both channels"
```

### 6.4 Example Monitoring Script

```bash
#!/bin/bash
# /opt/scripts/monitor-system.sh
# Run via cron every 5 minutes

LOG_FILE="/var/log/system-monitor.log"

# Temperature check
TEMP=$(cat /sys/class/hwmon/hwmon0/temp1_input 2>/dev/null)
TEMP_C=$((TEMP / 1000))

if [ $TEMP_C -gt 58 ]; then
    /usr/local/bin/notify-critical.sh "Temperature critical: ${TEMP_C}°C"
    echo "$(date): CRITICAL - Temp ${TEMP_C}°C" >> "$LOG_FILE"
elif [ $TEMP_C -gt 55 ]; then
    /usr/local/bin/notify-ntfy.sh "Temperature warning: ${TEMP_C}°C" "high"
    echo "$(date): WARNING - Temp ${TEMP_C}°C" >> "$LOG_FILE"
fi

# Disk space check
DISK_USED=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')

if [ $DISK_USED -gt 90 ]; then
    /usr/local/bin/notify-critical.sh "Disk space critical: ${DISK_USED}% used"
    echo "$(date): CRITICAL - Disk ${DISK_USED}%" >> "$LOG_FILE"
elif [ $DISK_USED -gt 75 ]; then
    /usr/local/bin/notify-ntfy.sh "Disk space warning: ${DISK_USED}% used" "default"
    echo "$(date): WARNING - Disk ${DISK_USED}%" >> "$LOG_FILE"
fi

# Docker containers health
UNHEALTHY=$(docker ps --filter "health=unhealthy" -q | wc -l)
if [ $UNHEALTHY -gt 0 ]; then
    CONTAINERS=$(docker ps --filter "health=unhealthy" --format "{{.Names}}" | tr '\n' ', ')
    /usr/local/bin/notify-critical.sh "Unhealthy containers: $CONTAINERS"
    echo "$(date): CRITICAL - Unhealthy: $CONTAINERS" >> "$LOG_FILE"
fi
```

**Add to crontab:**
```bash
# Edit crontab
crontab -e

# Add monitoring script (runs every 5 minutes)
*/5 * * * * /opt/scripts/monitor-system.sh
```

---

## 7. Testing Procedure

### Phase 1: Test Contact Points (Before Deployment)

**1. Test ntfy:**
```bash
# Manual test
curl -d "Test notification from terminal" http://lepotato.local:8080/homeserver

# Check ntfy logs
docker logs ntfy
```

**2. Test Telegram:**
```bash
# Manual test
curl -X POST "https://api.telegram.org/bot<TOKEN>/sendMessage" \
  -d "chat_id=<CHAT_ID>" \
  -d "text=Test from shell"
```

**3. Test Grafana Contact Points:**
- Go to Alerts & IRM → Alerting → Contact Points
- Click "Test" button next to each contact point
- Verify notifications arrive within 10 seconds
- Check logs if failures occur: `docker logs grafana`

### Phase 2: Test Alert Rules (Controlled Tests)

**1. Create Test Alert Rule:**
```yaml
Alert name: TEST - Always Firing
Folder: Testing
Evaluation interval: 1m
For: 0m

Query A:
  Data source: TestData
  Scenario: Random Walk

Condition:
  WHEN last() OF A IS ABOVE -99999  # Always true

Annotations:
  summary: This is a test alert
  description: If you receive this, alerting is working correctly

Labels:
  severity: warning
  test: true
```

**2. Verify Alert Fires:**
- Wait 1-2 minutes for evaluation
- Check Alerts & IRM → Alerting → Alert Rules
- Status should show "Firing"
- Verify notification received

**3. Delete Test Rule:**
- Once confirmed working, delete the test alert rule

### Phase 3: Simulate Real Alerts (Safe Tests)

**1. Test Temperature Alert (Safe Simulation):**
```bash
# Temporarily lower threshold in alert rule to 40°C
# Wait for alert to fire
# Restore threshold to 58°C
```

**2. Test Disk Space Alert:**
```bash
# Create large temporary file (safe)
cd /tmp
dd if=/dev/zero of=testfile bs=1G count=5  # Adjust size as needed
# Wait for alert
# Delete file: rm testfile
```

**3. Test Container Down Alert:**
```bash
# Stop a non-critical container temporarily
docker stop [test-container]
# Wait for alert (2-3 minutes)
# Restart container
docker start [test-container]
```

### Phase 4: Verify Deduplication

**1. Trigger same alert multiple times:**
- Ensure you only receive ONE notification per group interval
- Check Grafana → Alerting → Notifications to see grouping

**2. Verify Repeat Interval:**
- Leave alert in firing state
- Verify you receive reminder after configured interval (e.g., 4 hours)

### Phase 5: Test Alert Resolution

**1. Trigger alert, then resolve condition:**
- E.g., fill disk above threshold, then free space
- Verify you receive "Resolved" notification
- Check notification contains resolution timestamp

---

## 8. Alert Fatigue Mitigation

### 8.1 Grouping Strategy

**Group related alerts together:**
```
Group by: alertname, instance
Group wait: 30s     # Wait 30s to group similar alerts
Group interval: 5m  # Send grouped alerts every 5 minutes
```

**Example:** If 5 containers restart simultaneously, receive ONE notification with all 5 names, not 5 separate notifications.

### 8.2 Severity-Based Repeat Intervals

```
Critical: Repeat every 4 hours (if not resolved)
Warning: Repeat every 12 hours
Info: Do not repeat (one-time notification)
```

### 8.3 Mute Timings (Maintenance Windows)

**Create mute timing for planned maintenance:**
```
Navigate to: Alerts & IRM → Alerting → Mute Timings
Name: Weekend Maintenance
Time intervals:
  - Saturday 22:00 - Sunday 06:00
```

**Apply to notification policy:**
```
Notification Policy: [Your policy]
Mute timings: Weekend Maintenance
```

### 8.4 Silences (Temporary Suppression)

**When to use:**
- Known issues being worked on
- Testing/debugging scenarios
- Planned downtime

**How to create:**
```
Navigate to: Alerts & IRM → Alerting → Silences
Create silence:
  Matchers: alertname=DiskAlmostFull
  Duration: 2 hours
  Comment: Working on log cleanup
```

### 8.5 Alert Tuning Best Practices

1. **Start conservative, tune over time:**
   - Begin with higher thresholds
   - Lower thresholds as you understand normal behavior

2. **Use "For" duration to reduce flapping:**
   ```
   For: 5m  # Alert must be true for 5 minutes before firing
   ```

3. **Implement smart thresholds:**
   ```
   Bad:  CPU > 80%  (may trigger during normal bursts)
   Good: CPU > 80% for 5 minutes AND increasing trend
   ```

4. **Provide actionable context:**
   ```
   Bad description:  "Disk full"
   Good description: "Disk {{ $labels.device }} is {{ $value }}% full.
                     Run: docker system prune -a --volumes"
   ```

5. **Review alert effectiveness weekly:**
   - Which alerts fired?
   - Were they actionable?
   - Any false positives?
   - Adjust thresholds accordingly

---

## 9. Cost Analysis

### One-Time Setup Costs

| Item | Cost | Notes |
|------|------|-------|
| ntfy.sh (self-hosted) | $0 | Free and open source |
| Telegram Bot | $0 | Free API, no limits |
| Discord Webhook | $0 | Free, unlimited webhooks |
| Grafana Alerting | $0 | Included with Grafana OSS |
| **Total Setup** | **$0** | |

### Optional Paid Services (if desired)

| Service | Cost | Notes |
|---------|------|-------|
| Pushover | $4.99 one-time | Per platform (iOS/Android) |
| Twilio SMS | $0.0075/msg | ~$1-5/month for 100-500 alerts |
| PagerDuty | $21/user/mo | Enterprise-grade, overkill for homelab |
| OpsGenie | $9/user/mo | Also overkill for homelab |

### Recommended Budget

**Free Tier (Recommended for Homelab):**
- ntfy.sh (self-hosted) + Telegram
- Total monthly cost: **$0**
- Reliability: High (two independent channels)

**Enhanced Tier (Optional):**
- ntfy.sh + Telegram + Pushover
- Total cost: $4.99 one-time + $0/month
- Benefit: Slightly better mobile app experience with Pushover

### Resource Costs (Le Potato)

| Service | RAM Usage | CPU Usage | Storage |
|---------|-----------|-----------|---------|
| ntfy | 50-128 MB | <1% idle, 2-5% when sending | ~50 MB |
| Grafana alerting | (included in Grafana) | Negligible | <10 MB |
| Telegram (API only) | 0 MB | 0% | 0 MB |

**Total Additional Resource Usage:** ~128 MB RAM, ~60 MB storage

---

## 10. ARM/Resource Constraints

### 10.1 ntfy on ARM

**Compatibility:** ✅ Excellent
- Official Docker images for armv6, armv7, arm64
- Proven deployments on Raspberry Pi (same ARM architecture as Le Potato)
- Minimal resource usage (perfect for low-power ARM boards)

**Performance on Le Potato (expected):**
- 2GB RAM system: ntfy uses ~2-3% of total RAM
- Quad-core CPU: Negligible CPU usage (<1% most of time)
- I/O: Minimal (writes to cache only when notifications sent)

### 10.2 Grafana Alerting on ARM

**Compatibility:** ✅ Excellent
- Grafana official ARM64 Docker images available
- Alerting engine is part of Grafana (already running)
- No additional resource overhead beyond base Grafana

**Performance Considerations:**
- Alert evaluation runs in Grafana scheduler
- Minimal CPU impact (1-2% during evaluation cycles)
- No significant memory overhead
- Tested extensively on Raspberry Pi 4 (similar specs to Le Potato)

### 10.3 Telegram/Discord/Email

**Resource Usage:** ✅ Negligible
- API-based services (no local hosting)
- Only HTTP requests when alerts fire
- Network bandwidth: <1 KB per alert

### 10.4 Services to AVOID on ARM

❌ **Prometheus Alertmanager** (standalone)
- Adds unnecessary complexity when Grafana alerting exists
- Additional memory overhead (~100-200MB)
- Redundant with Grafana's built-in alerting

❌ **Heavy notification services** (PagerDuty agent, etc.)
- Designed for enterprise infrastructure
- Unnecessary resource consumption

### 10.5 Recommended Configuration for Le Potato

**Monitoring Stack Resource Budget:**
```
Grafana:        ~300-400 MB RAM
Prometheus:     ~200-300 MB RAM
Loki:           ~150-200 MB RAM
ntfy:           ~50-100 MB RAM
Node Exporter:  ~20-30 MB RAM
cAdvisor:       ~50-80 MB RAM
-----------------------------------
Total:          ~770-1110 MB RAM (38-55% of 2GB)

Remaining for other services: ~1GB
```

**Conclusion:** ✅ Le Potato has sufficient resources for full monitoring + alerting stack

---

## 11. Implementation Checklist

### Phase 1: Foundation (Week 1)

- [ ] Deploy ntfy container
- [ ] Create Telegram bot via BotFather
- [ ] Configure Grafana contact points (ntfy + Telegram)
- [ ] Test both contact points from Grafana UI
- [ ] Create notification policies (critical vs warning)

### Phase 2: Critical Alerts (Week 1-2)

- [ ] Create alert: Service Down (Pi-hole)
- [ ] Create alert: Service Down (Tailscale)
- [ ] Create alert: Service Down (Docker daemon)
- [ ] Create alert: Disk Space Critical (>90%)
- [ ] Create alert: Temperature Critical (>58°C)
- [ ] Create alert: SD Card I/O Errors
- [ ] Test each alert with controlled simulation

### Phase 3: Warning Alerts (Week 2-3)

- [ ] Create alert: Container Restart Excessive
- [ ] Create alert: Log Volume High
- [ ] Create alert: Disk Space Warning (75-90%)
- [ ] Create alert: Temperature Warning (55-58°C)
- [ ] Test warning alerts

### Phase 4: Shell Script Integration (Week 3)

- [ ] Create `/usr/local/bin/notify-ntfy.sh`
- [ ] Create `/usr/local/bin/notify-telegram.sh`
- [ ] Create `/usr/local/bin/notify-critical.sh` (dual notification)
- [ ] Create `/opt/scripts/monitor-system.sh`
- [ ] Add monitoring script to crontab (every 5 minutes)
- [ ] Test shell script notifications

### Phase 5: Fine-Tuning (Week 4+)

- [ ] Monitor for false positives (adjust thresholds)
- [ ] Implement mute timings if needed
- [ ] Set up alert grouping/deduplication
- [ ] Review alert effectiveness weekly
- [ ] Document custom alerts in runbook

### Optional Enhancements

- [ ] Deploy Uptime Kuma for service availability monitoring
- [ ] Set up email contact point for daily summaries
- [ ] Configure Discord webhook if desired
- [ ] Add Pushover if budget allows ($4.99)

---

## 12. Troubleshooting

### ntfy Notifications Not Received

**Check ntfy is running:**
```bash
docker ps | grep ntfy
docker logs ntfy
```

**Test ntfy directly:**
```bash
curl -d "Direct test" http://localhost:8080/homeserver
```

**Check firewall/network:**
```bash
# From another device on network
curl -d "Network test" http://lepotato.local:8080/homeserver
```

**Verify mobile app subscription:**
- Open ntfy app
- Subscribe to: `http://lepotato.local:8080/homeserver`
- Or if using ntfy.sh: `ntfy.sh/homeserver`

### Telegram Notifications Not Received

**Verify bot token:**
```bash
curl https://api.telegram.org/bot<TOKEN>/getMe
# Should return bot information
```

**Check chat ID:**
```bash
# Message your bot, then:
curl https://api.telegram.org/bot<TOKEN>/getUpdates
# Look for your message and verify chat ID
```

**Test direct send:**
```bash
curl -X POST "https://api.telegram.org/bot<TOKEN>/sendMessage" \
  -d "chat_id=<CHAT_ID>" \
  -d "text=Manual test"
```

### Grafana Alerts Not Firing

**Check alert rule evaluation:**
- Navigate to alert rule
- Look at "Last evaluated" timestamp
- Check "State" (Normal, Pending, Firing)

**Check query data:**
- Click "Run queries" button in alert rule
- Verify data source returns expected values
- Check threshold condition logic

**Check notification policy:**
- Verify labels match (severity=critical)
- Check contact points are configured
- Look for mute timings or silences

**Check Grafana logs:**
```bash
docker logs grafana | grep -i alert
docker logs grafana | grep -i notif
```

### Alert Fatigue Issues

**Too many alerts?**
- Increase "For" duration (e.g., 2m → 5m)
- Raise thresholds slightly
- Implement grouping (group by alertname)
- Increase repeat interval

**Missing important alerts?**
- Lower "For" duration for critical alerts
- Verify contact points are tested and working
- Check for silences or mute timings
- Review notification policy routing

### Performance Issues

**ntfy using too much RAM?**
```bash
# Check actual usage
docker stats ntfy

# Clear cache if needed
docker exec ntfy rm -rf /var/cache/ntfy/*
docker restart ntfy
```

**Too many alert evaluations?**
- Increase evaluation interval (1m → 2m or 5m)
- Reduce number of active alert rules
- Optimize Prometheus queries (use recording rules)

---

## 13. Future Considerations

### Scaling Beyond Single Server

**If expanding to multiple servers:**
- Deploy ntfy as centralized notification hub
- All servers send to same ntfy instance
- Use labels/topics to identify source: `homeserver1-critical`, `homeserver2-critical`
- Grafana can federate alerts from multiple Prometheus instances

### Advanced Features to Explore

**After basic alerting is stable:**
- **Grafana OnCall** (incident management)
- **Alert dependencies** (suppress secondary alerts when primary fires)
- **Dynamic thresholds** (anomaly detection with ML)
- **Custom templates** (rich notification formatting)
- **Webhook integrations** (trigger automation on alerts)

### Integration with Home Assistant

**If using Home Assistant:**
- ntfy integrates natively with HA
- Create HA automations triggered by notifications
- Send alerts to HA dashboard
- Use HA notification services alongside ntfy

---

## 14. Sources and References

### Official Documentation

1. **Grafana Alerting:**
   - https://grafana.com/docs/grafana/latest/alerting/
   - https://grafana.com/docs/grafana/latest/alerting/best-practices/
   - https://grafana.com/blog/2024/05/14/grafana-alerting-new-tools-to-resolve-incidents-faster-and-avoid-alert-fatigue/

2. **ntfy.sh:**
   - https://ntfy.sh/
   - https://docs.ntfy.sh/install/
   - https://docs.ntfy.sh/integrations/

3. **Telegram Bot API:**
   - https://core.telegram.org/bots
   - https://grafana.com/docs/grafana/latest/alerting/configure-notifications/manage-contact-points/integrations/configure-telegram/

4. **Discord Integration:**
   - https://grafana.com/docs/grafana/latest/alerting/configure-notifications/manage-contact-points/integrations/configure-discord/

5. **Pushover:**
   - https://pushover.net/
   - https://pushover.net/api

### Community Resources

6. **Home Lab Monitoring:**
   - https://pimylifeup.com/raspberry-pi-ntfy/
   - https://blog.alexsguardian.net/posts/2023/09/12/selfhosting-ntfy/
   - https://www.xda-developers.com/set-up-self-hosted-notification-service/

7. **Docker Container Monitoring:**
   - https://dev.to/kanani_nirav/how-to-monitor-and-alert-docker-container-status-on-ec2-for-high-availability-bmj
   - https://medium.com/cloud-native-daily/how-to-monitor-and-alert-docker-container-status-on-ec2-for-high-availability-475b0b65e04

8. **Uptime Kuma:**
   - https://uptimekuma.org/
   - https://betterstack.com/community/guides/monitoring/uptime-kuma-guide/

9. **Raspberry Pi / ARM Compatibility:**
   - https://0xmm.in/posts/ntfy/
   - https://www.xda-developers.com/7-self-hosted-services-i-use-that-can-run-perfectly-on-a-raspberry-pi/

10. **Alert Fatigue and Best Practices:**
    - https://drdroid.io/engineering-tools/grafana-alerting-advanced-alerting-configurations-best-practices
    - https://community.grafana.com/t/struggling-with-alert-fatigue-looking-for-best-practices-and-tool-recommendations/147253
    - https://reintech.io/blog/grafana-alerting-notifications-best-practices

### Comparison Resources

11. **ntfy vs Pushover:**
    - https://opensourcealternative.to/alternativesto/pushover
    - https://www.saashub.com/compare-pushover-vs-ntfy
    - https://github.com/mailcow/mailcow-dockerized/issues/6473

12. **Grafana vs Uptime Kuma:**
    - https://cloudpap.com/blog/uptime-kuma-vs-grafana/
    - https://cloudpap.com/blog/uptime-kuma-and-grafana/

---

## 15. Conclusion

### Recommended Solution Summary

**Primary Notification:** ntfy (self-hosted)
- Free, open source, ARM-compatible
- Perfect for homelab use case
- Low resource usage (~128MB RAM)
- Works from Grafana and shell scripts

**Backup Notification:** Telegram Bot
- Free, reliable, excellent mobile app
- Independent of self-hosted infrastructure
- Ensures alerts reach you even if server has issues

**Cost:** $0 (both services are free)

**Reliability:** High (dual notification channels)

**Ease of Setup:** Medium (2-3 hours initial setup, well-documented)

**Maintenance:** Low (minimal ongoing maintenance required)

### Confidence Level: **HIGH**

**Reasons:**
- Extensive documentation for all recommended services
- Proven deployments on ARM/Raspberry Pi systems
- Active community support and examples
- Free/open source reduces risk and cost
- Multiple verification sources confirm compatibility
- Best practices well-established for home lab monitoring

### Next Steps

1. Proceed with implementation using checklist in Section 11
2. Start with Phase 1 (ntfy + Telegram setup)
3. Test thoroughly before deploying production alerts
4. Monitor and tune over 2-4 weeks
5. Document any custom configurations in runbook

---

**End of Report**
