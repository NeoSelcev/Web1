# � web1 Projects Platform

Comprehensive web hosting platform running on dedicated hardware with automatic recovery, intelligent alerting, and remote management featuring advanced system diagnostics, centralized Telegram notifications, and intelligent failure detection.

## 🔐 Security & Access

### External Access (Cloudflare Tunnel)

**Production Services** - https://web1.1ddjrkbeu4e472wsits1.dpdns.org
- � **Web Projects**: `/` - Main web projects hosting
- 📄 **Login Page**: `/` - Basic Authentication form

**Test Page** - https://web1-test.1ddjrkbeu4e472wsits1.dpdns.org
- ✅ GitHub OAuth authentication
- 24-hour session duration

**Security Features:**
- ✅ Zero port forwarding - No open ports on router
- ✅ Cloudflare Tunnel with automatic SSL/TLS
- ✅ Basic Authentication (nginx-auth proxy)
- ✅ DDoS protection at edge level
- ✅ Hidden origin IP address

**Credentials:** See `credentials.txt` for access details

### VPN Access (Tailscale - SSH Only)

**SSH via Tailscale VPN:**
```bash
ssh web1-vpn      # → Tailscale IP (to be configured)
ssh web1          # → Local network IP (to be configured)
```

**Note:** Tailscale Serve/Funnel for web services has been **disabled**.
Web access is now exclusively through Cloudflare Tunnel.

### Architecture

```
Internet → Cloudflare Edge → Cloudflare Tunnel → web1-proxy (Basic Auth) → web-main (80)
```

## 🎯 System Capabilities

### 🔍 **Comprehensive System Diagnostics**
Advanced system health monitoring with **79 comprehensive checks** accessible via unified aliases:

```bash
# System diagnostic aliases (all point to system-diagnostic.sh)
sysdiag    # Full system diagnostics (79 checks)
diag       # Same as sysdiag
diagnostic # Same as sysdiag
syscheck   # Quick essential checks (--quick)
fullcheck  # Full diagnostic with detailed output (--full)
```

**Remote SSH Usage:**
```bash
# Updated remote commands
ssh web1 sysdiag       # Full diagnostics on main system
ssh web1 syscheck      # Quick check on main system
ssh web1 fullcheck     # Full detailed check on main system
```

**Diagnostic Coverage:**
- 🖥️ **System Resources** - Memory, disk, CPU load, temperature
- 🌐 **Network Connectivity** - Internet, gateway, DNS, interfaces
- 🐳 **Docker Services** - Daemon, containers, compose files
- 🏠 **WEB1 Monitoring** - Watchdog, notifier, scripts, timers
- 📊 **Service Availability** - Port checks (8123, 1880, 9000, 8080) using bash `/dev/tcp`
- 📝 **Log Analysis** - File sizes, recent entries, state files
- 🔒 **Security Status** - SSH, firewall, updates
- ⚡ **Performance Tests** - Disk speed, memory stress tests

**Detailed Diagnostic Coverage (79 checks total):**

**🖥️ Basic System Info (6 checks)**
- Hostname, Uptime, Kernel, OS, Architecture, CPU Model

**💾 System Resources (8 checks)**
- Memory usage (total/used/available), disk space, CPU load, temperature monitoring

**🌐 Extended Network Diagnostics (11 checks)**
- Internet access, gateway, DNS, network interfaces, WiFi/Ethernet status, IP addresses

**🐳 Docker Services (12+ checks)**
- Docker daemon, version, info, containers, compose configuration

**🔗 Tailscale VPN Diagnostics (8+ checks)**
- Daemon status, connection, node info, peers, WEB1 accessibility via VPN

**🔍 WEB1 Monitoring Services (12+ checks)**
- Systemd services, timers, scripts, configuration validation

**📊 Log Analysis (6+ checks)**
- Log files, sizes, recent entries, state files

**🚪 Service Availability (4+ checks)**
- WEB1, Node-RED, Portainer, Zigbee2MQTT port checks

**📈 Recent Failures Analysis (4+ checks)**
- Failure logs, notification statistics, throttling status

**🔒 Enhanced Security (8+ checks)**
- SSH configuration, firewall, fail2ban, file permissions, security updates

**⚡ Performance Testing (2+ checks)**
- Disk write speed, memory stress test

**Latest Performance Results:**
- ✅ **66/79 checks passed (83%)**
- ⚠️ **12 warnings** (mostly security recommendations)
- ❌ **1 error** (minor issue)

**Smart Reporting:**
- 🎨 Color-coded results (✓ PASS, ✗ FAIL, ⚠ WARN)
- 📊 Statistical summary with percentage scores
- 📋 Detailed reports saved to `/tmp/system_diagnostic_YYYYMMDD_HHMMSS.txt`
- 🔄 Automatic system health assessment

### 🔧 **Intelligent Recovery & Monitoring**
- **20-Point Health Monitoring**: Network, resources, services, remote access, system health
- **Auto-restart**: Failed containers and network interfaces
- **Smart throttling**: Prevents notification spam with configurable intervals
- **Failure analysis**: Context-aware error categorization and response

### 📱 **Advanced Telegram Integration**
Centralized **telegram-sender v1.0** service with topic-based routing and intelligent throttling:

**Key Features:**
- 🎯 **Topic-oriented sending** - automatic topic detection by ID
- 🔄 **Retry mechanism** - 3 sending attempts with 2-second delay
- 📝 **Detailed logging** - tracking senders, statuses, errors
- ⚙️ **Flexible configuration** - separate config file with full settings
- 🔒 **Security** - token validation and message verification
- 📊 **Performance Metrics** - sender tracking and delivery statistics

**Supported Topics:**
- 🏠 **SYSTEM (ID: 2)** - System messages and general information
- 🚨 **ERRORS (ID: 10)** - Critical errors and system failures
- 📦 **UPDATES (ID: 9)** - Package and Docker image updates
- 🔄 **RESTART (ID: 4)** - Reboots and service restarts
- 🔍 **SYSTEM_DIAGNOSTIC (ID: 123)** - System diagnostic reports and health checks
- 💾 **BACKUP (ID: 131)** - Backup reports and status updates

**Notification Priorities:**
- 🔴 **CRITICAL**: High temperature (>70°C), web1 unreachable
- 🟠 **IMPORTANT**: Docker container failures, network issues
- 🟡 **WARNING**: High system load, slow network
- 🟢 **INFO**: Service recovery, successful restarts
- 🌙 **NIGHTLY REPORTS**: Daily system status and update summaries

**Usage:**
```bash
# Direct call with topic
telegram-sender.sh "Message" "10"  # To ERRORS topic
telegram-sender.sh "System diagnostic completed" "123"  # To SYSTEM_DIAGNOSTIC topic
telegram-sender.sh "Backup completed successfully" "131"  # To BACKUP topic

# From monitoring scripts
"$TELEGRAM_SENDER" "$message" "2"    # To SYSTEM topic
"$TELEGRAM_SENDER" "$diagnostic_report" "123"  # To SYSTEM_DIAGNOSTIC topic
"$TELEGRAM_SENDER" "$backup_status" "131"      # To BACKUP topic
```

**Service Files:**
```
/usr/local/bin/telegram-sender.sh     # Main script
/etc/telegram-sender/config           # Configuration
/var/log/telegram-sender.log          # Sending logs
/etc/logrotate.d/telegram-sender      # Log rotation
```

**Architecture Benefits:**
- 🎯 **Single Point of Configuration** - All Telegram settings in `/etc/telegram-sender/config`
- 📝 **Centralized Logging** - Unified logs in `/var/log/telegram-sender.log`
- 🔄 **Retry & Error Handling** - Built-in resilience with 3 attempts per message
- 🏗️ **Topic-Based Routing** - Automatic message categorization by service type
- 📊 **Performance Metrics** - Sender tracking and delivery statistics

**Refactoring Results:**
- ✅ **Eliminated 65+ lines of duplicate code** across monitoring services
- ✅ **Reduced 14 individual curl calls** to single service invocations
- ✅ **Simplified configuration management** - no more token duplication
- ✅ **Enhanced error handling** - centralized retry logic and logging

## 📋 System Specifications

### **Hardware & OS**
- **OS**: Debian GNU/Linux 12 (bookworm), Kernel 6.1.0-37+
- **Storage**: 32GB eMMC, optimized for performance
- **Memory**: 2GB DDR3L, efficient resource utilization
- **Network**: 192.168.1.22 (local), 100.80.189.88 (VPN Tailscale)

### **Active Services Stack**
- **web1**: Port 8123 (ghcr.io/home-assistant/home-assistant:stable)
- **Node-RED**: Port 1880 (nodered/node-red:latest)
- **Tailscale VPN**: 100.80.189.88 with public HTTPS access
- **Docker**: Latest version (container orchestration)
- **SSH**: Port 22 (ed25519 key authentication)

### **Docker Stack**
```yaml
services:
  web1:
    image: web1/home-assistant:stable
    network_mode: host
    ports: 8123
    logging:
      max-size: "10m"
      max-file: "7"

  nodered:
    image: nodered/node-red:latest
    ports: 1880
    logging:
      max-size: "10m"
      max-file: "7"
```

## 🌐 Cloudflare Tunnel Security

### **Cloudflared Service**

- **Type**: Docker container (cloudflared-tunnel)
- **Image**: `cloudflare/cloudflared:latest`
- **Config**: `/opt/web1/cloudflared/config.yml`
- **Function**: Secure tunnel to Cloudflare edge network
- **Benefits**: Zero-trust access, automatic SSL, DDoS protection

### **Security Features**

- **Tunnel Protocol**: QUIC/HTTP2 over TLS 1.3
- **IP Protection**: Real server IP hidden from public
- **Automatic SSL**: Certificate management via Cloudflare
- **DDoS Mitigation**: Edge-level protection
- **Access Control**: Zero Trust authentication available
- **Geographic Distribution**: Multiple edge locations

### **Tunnel Configuration**

- **Tunnel ID**: `[TO BE CONFIGURED]`
- **Domain**: `[YOUR-DOMAIN]`
- **Service**: Routes traffic to local services securely
- **Monitoring**: Health checks and connection status

### **Ingress Configuration**

**Configuration File**: `/opt/web1/cloudflared/config.yml`

```yaml
tunnel: [TUNNEL_ID]
credentials-file: /etc/cloudflared/[TUNNEL_ID].json

ingress:
  # web1 Admin Panel (with Basic Auth + Cloudflare Access)
  - hostname: web1.[YOUR-DOMAIN]
    service: http://web1-proxy:8080
  # Test web page via Docker network
  - hostname: test.[YOUR-DOMAIN]
    service: http://test-web:80
  # Catch-all rule (required)
  - service: http_status:404
```

**Ingress Rules**:
- **Docker Networking**: Routes to container names (web1-proxy:8080, test-web:80)
- **Hostname Routing**: Specific subdomains route to different services
- **Fallback Rule**: 404 response for undefined routes
- **Container Integration**: Direct connection to Docker containers
- **Validation**: `docker exec cloudflared-tunnel cloudflared tunnel ingress validate`

### **Dual Authentication Security**

**Multi-Layer Protection for web1 Admin Panel**:

The web1 admin interface is protected by **two layers of authentication**:

1. **Cloudflare Access (OAuth Layer)**
   - GitHub OAuth integration
   - Google OAuth integration (optional)
   - Organization/email-based access control
   - Session management and device trust
   - Zero Trust architecture

2. **HTTP Basic Authentication (Application Layer)**
   - nginx-proxy with Basic Auth
   - Username: `admin`
   - Password: Configured in `/opt/web1/nginx-auth/.htpasswd`
   - Additional security layer after OAuth

**Authentication Flow**:
```
User → Cloudflare Access (GitHub/Google OAuth)
     → HTTP Basic Auth (nginx-proxy)
     → web1 Admin Panel
```

**nginx-proxy Configuration**:
- **Container**: `web1-proxy`
- **Image**: `nginx:alpine`
- **Config**: `/opt/web1/nginx-auth/nginx.conf`
- **Credentials**: `/opt/web1/nginx-auth/.htpasswd`
- **Port**: 8080 (internal, accessed via Cloudflare Tunnel)
- **Proxy Target**: `http://web1:8123`

**Access URLs**:
- **Admin Panel**: `https://web1.[YOUR-DOMAIN]` (dual auth required)
- **Test Page**: `https://test.[YOUR-DOMAIN]` (public)

**Login Methods**:

Due to Chrome/Chromium browser limitations with Basic Auth over Cloudflare tunnels, a custom login page is provided:

1. **Custom Login Form** (Recommended):
   - Navigate to `https://web1.[YOUR-DOMAIN]`
   - Enter HTTP Basic Auth credentials in the web form:
     - Username: `admin`
     - Password: `[YOUR-PASSWORD]`
   - Then login to web1 with your WEB1 credentials

2. **URL with Credentials** (Alternative):
   - `https://admin:[YOUR-PASSWORD]@web1.[YOUR-DOMAIN]`
   - Use when browser doesn't show authentication dialog

3. **Standard Basic Auth Dialog** (Firefox/Safari):
   - Some browsers may show the standard authentication prompt
   - Enter HTTP Basic Auth: `admin` / `[YOUR-PASSWORD]`
   - Then web1 login with your WEB1 credentials

**Note**: Chrome often doesn't display the Basic Auth dialog for HTTPS connections through Cloudflare. The custom login form solves this issue with a user-friendly interface.

**Cloudflare Access OAuth Setup**:

For complete dual authentication setup with GitHub/Google OAuth, see the **Installation Plan**: `docs/install_plan.md` (section "Setup Cloudflare Tunnel").

OAuth Applications Configuration:
- **GitHub OAuth**: Create at https://github.com/settings/developers
- **Google OAuth**: Create at https://console.cloud.google.com/apis/credentials
- **Cloudflare Zero Trust**: Configure at https://one.dash.cloudflare.com

Credentials Location:
- **In Project**: `docs/cloudflare-credentials.txt` (to be created)
- **On Server**: `/opt/web1/credentials/cloudflare-credentials.txt`

### **Troubleshooting**

**502 Bad Gateway Error**:

If you see "502 Bad Gateway" after authentication, this means nginx-proxy can't reach web1 container.

**Solution**:
```bash
# Check container status
ssh web1 "docker ps | grep -E 'web1|web1-proxy'"

# Restart nginx-proxy to apply changes
ssh web1 "docker restart web1-proxy"

# Check nginx logs
ssh web1 "docker logs web1-proxy"

# Verify containers are healthy
ssh web1 "docker ps"
```

### **Security Setup**

**Access Security**:
- **Origin Protection**: Server IP completely hidden
- **Certificate Management**: Automatic SSL/TLS via Cloudflare
- **No Inbound Ports**: Only outbound connections from server
- **Firewall Friendly**: No need to open ports in firewall
- **Dual Authentication**: OAuth + Basic Auth for admin access
- **Zero Trust**: Cloudflare Access with identity verification

**Monitoring Integration**:
- **Container Health**: Monitored by web1-watchdog Docker checks
- **Connection Status**: Tracked in system diagnostics via container logs
- **Public Access**: Validated via HTTPS health checks
- **Tunnel Metrics**: Connection count and uptime monitoring via Docker stats

## 🔧 System Configuration

### **System Performance Optimizations**

Comprehensive performance optimizations for x86 server (Dell Wyse 3040):

#### Traditional Swap Configuration
Basic 2GB swap file for baseline performance:

```bash
# Create 2GB swap file
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

# Add to fstab for auto-mount
echo '/swapfile none swap sw 0 0' >> /etc/fstab

# Verify result
free -h
swapon --show
```

#### ZRAM Setup (Advanced)
Compressed swap in RAM with 3x compression ratio (applied October 28, 2025):

```bash
# Install zram-tools
apt install -y zram-tools

# Configure /etc/default/zramswap
PERCENTAGE=50      # 256MB (50% of 1.8GB RAM)
ALGO=lz4          # Fast compression for x86
PRIORITY=10       # Higher than file swap

# Result: +492Mi available RAM (+61% improvement)
```

#### Kernel Tuning
Optimizations for x86 systems with ZRAM (`/etc/sysctl.conf`):

```bash
# ZRAM optimizations (x86 server)
vm.swappiness=100                # Prefer ZRAM swap
vm.vfs_cache_pressure=50         # Keep cache longer
vm.dirty_ratio=10                # Dirty page writeback
vm.dirty_background_ratio=5      # Background writeback
vm.min_free_kbytes=16384         # Reserve 16MB free

# Network optimizations (for Docker)
net.core.netdev_max_backlog=2500
net.core.somaxconn=1024

# Disable IPv6 if not used
net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6=1
```

#### Transparent Huge Pages & CPU Optimization

```bash
# Reduce memory fragmentation for Docker
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
echo madvise > /sys/kernel/mm/transparent_hugepage/defrag

# CPU Governor: performance mode for maximum responsiveness
echo performance > /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
# Made permanent via systemd service: cpu-performance.service
```

#### Disk Optimizations

```bash
# /etc/fstab - noatime and nodiratime (reduces HDD writes)
UUID=a446003f-8e9f-43c1-a02a-51a201ec0b81 / ext4 noatime,nodiratime,errors=remount-ro 0 1

# tmpfs for /tmp (10-30x faster)
tmpfs /tmp tmpfs defaults,noatime,mode=1777,size=200M 0 0
```

#### Docker Configuration

```bash
# Optimized /etc/docker/daemon.json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"      # Reduced from 7 to 3
  },
  "storage-driver": "overlay2"
}
```

#### Performance Results

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Available RAM | 808 Mi | 1.3 Gi | **+61% (+492Mi)** |
| Swap Speed | 50-100 MB/s | 2000+ MB/s | **20-40x faster** |
| CPU Performance | Variable | Max (always) | **10-20% faster** |
| /tmp Operations | HDD | RAM (tmpfs) | **10-30x faster** |
| Disk I/O | Normal | Optimized | **15-20% faster** |

### **Log Rotation Configuration**
Automated log management prevents disk space exhaustion:

**Logrotate Configurations:**
- `/etc/logrotate.d/web1-monitoring` - monitoring system logs
- `/etc/logrotate.d/web1` - web1 logs (50MB → rotate, 7 files)
- `/etc/logrotate.d/fail2ban` - Fail2ban logs (52 weeks retention)
- `/etc/logrotate.d/ufw` - UFW firewall logs (30 days retention)
- `/etc/systemd/journald.conf` - systemd journal limits (500MB max)

**High-frequency logs (every 2-5 minutes):**
```
/var/log/web1-watchdog.log, /var/log/web1-failure-notifier.log
├─ Size: 5MB → rotate
├─ Archive: 10 files (50MB total limit)
├─ Frequency: daily
└─ Compression: enabled
```

**Medium-frequency logs:**
```
/var/log/web1-failures.log, /var/log/web1-reboot.log
├─ Size: 10MB → rotate
├─ Archive: 5 files
└─ Frequency: weekly
```

### **Log Management Configuration**

**Multi-level log management system:**

1. **Docker logs** (`/etc/docker/daemon.json`):
   - Global limits: 10MB per file, 7 archived files per container
   - Total Docker logs: ~140MB (WEB1 + NodeRED)

2. **Application logs** (logrotate):
   - WEB1 monitoring logs: 5-20MB rotation limits with 10-3 file retention
   - web1 logs: 50MB rotation with 7 day retention
   - Automated rotation: daily at 00:00 UTC via systemd timer

3. **System logs** (systemd journal):
   - Limited to 500MB total (down from potential 1.5GB+)
   - 30-day retention with compression

**Log management commands:**

```bash
web1-monitoring-control log-sizes      # Check all log sizes
web1-monitoring-control rotate-logs    # Force log rotation
web1-monitoring-control clean-journal  # Clean systemd journal
```

**Automatic rotation:** `logrotate.timer` runs daily at midnight (systemd, not cron)

## 🔧 Monitoring Services

### **Service Schedule & Performance**

| Service | Frequency | Boot Delay | Purpose |
|---------|-----------|------------|----------|
| **web1-watchdog** | 2 minutes | 30 seconds | 20-point system health monitoring |
| **web1-failure-notifier** | 5 minutes | 1 minute | Telegram alerts & auto-recovery with smart throttling |
| **nightly-reboot** | Daily 03:30 | - | Maintenance reboot with health report |
| **update-checker** | Weekdays 09:00 ±30min | - | System/Docker update analysis |

### **web1-failure-notifier - Advanced Features**

**Smart Throttling System:**
Intelligent event-type based throttling that replaces generic limits with priority-based quotas:

- 🔴 **Critical Events** (HA_SERVICE_DOWN, MEMORY_CRITICAL): 20 events/30min
- 🟡 **High Priority** (HIGH_LOAD, CONNECTION_LOST): 10 events/30min
- 🟠 **Warnings** (MEMORY_WARNING, DISK_WARNING): 5 events/30min
- 🔵 **Info Events** (other): 3 events/30min
- ⏰ **Rolling Window** - 30-minute sliding window with automatic cleanup
- 🔄 **Type Independence** - Different event types don't block each other
- 🛡️ **Dual Protection** - Smart + legacy throttling for compatibility

**Timestamp-Based Processing:**
- ✅ **Rotation Independence** - Works regardless of log file rotation, truncation, or recreation
- ✅ **Duplicate Prevention** - Processes only events newer than last processed timestamp
- ✅ **Perfect Accuracy** - Based on actual event time, not file structure
- ✅ **Performance Boost** - Reduced processing time from 60s timeout to <1s execution

**State Files:**
```
/var/lib/web1-failure-notifier/
├── last_timestamp.txt        # Unix timestamp of last processed event
├── smart_throttle_history.txt # Smart throttling event history with priorities
├── position.txt              # Legacy: Last processed line number (kept for compatibility)
├── metadata.txt              # File metadata for rotation detection (size:ctime:mtime:hash)
├── throttle.txt              # Legacy: Timestamp tracking for notification throttling
└── hashes.txt                # Legacy hash storage (kept for compatibility)
```

### **Additional Components:**
- **Nightly Reboot Service**: Scheduled maintenance reboot at 3:30 AM with enhanced logging
- **Update Checker Service**: Weekday update analysis at 9:00 AM (±30min randomization)
- **Required System Packages**: bc, wireless-tools, dos2unix, curl, htop installed
- **Complete Service Suite**: 4 monitoring services with proper dependencies

### **Monitoring Coverage (20 Checks)**

#### **Network & Connectivity (4)**
- Internet connectivity, gateway reachability, network interface status, WiFi signal strength

#### **System Resources (4)**
- Memory availability, disk space, CPU temperature, system load average

#### **Services & Containers (3)**
- Docker containers health, WEB1/Node-RED port availability, critical systemd services

#### **Remote Access (3)**
- SSH accessibility, Tailscale VPN status, public HTTPS access (Funnel)

#### **System Health (6)**
- SD card errors, power supply/throttling, NTP sync, log sizes, WEB1 database integrity, swap usage

## 📝 Centralized Logging System

### **Overview**

All monitoring services use unified centralized logging through `logging-service.sh v1.1`:

**Key Features:**
- 🎯 **Unified Format** - Consistent log structure across all services
- 🔧 **Wrapper Functions** - Simple `log_debug()`, `log_info()`, `log_warn()`, `log_error()`, `log_critical()`
- ⚙️ **Service Name Auto-detection** - Uses `SCRIPT_NAME` variable for automatic identification
- 📊 **Structured Logging** - JSON support for metrics and extra data
- 🔄 **Backward Compatible** - Optional config file, works with defaults

**Log Format:**
```
YYYY-MM-DD HH:MM:SS [LEVEL] [service-name] [PID:12345] [caller] Message text
```

**Integrated Services (11 total):**
- ✅ `web1-watchdog.sh` - System health monitoring
- ✅ `web1-failure-notifier.sh` - Failure detection and recovery
- ✅ `telegram-sender.sh` - Telegram notifications
- ✅ `web1-backup.sh` - Backup operations
- ✅ `nightly-reboot.sh` - Scheduled reboots
- ✅ `update-checker.sh` - Update detection
- ✅ `system-diagnostic.sh` - System diagnostics
- ✅ `system-diagnostic-startup.sh` - Boot diagnostics
- ✅ `boot-notifier.sh` - Boot notifications
- ✅ `web1-monitoring-control.sh` - Service management
- ✅ `telegram-fail2ban-notify.sh` - Security alerts

**Usage in Scripts:**
```bash
#!/bin/bash
SCRIPT_NAME="my-service"  # Auto-detected in logs

LOGGING_SERVICE="/usr/local/bin/logging-service.sh"
if [[ -f "$LOGGING_SERVICE" ]]; then
    source "$LOGGING_SERVICE" 2>/dev/null
fi

# Use wrapper functions
log_info "Service started successfully"
log_warn "High memory usage detected"
log_error "Connection failed"
log_debug "Debug information"
log_critical "Critical system failure"
```

**Configuration (optional):**
```bash
# /etc/logging-service/config
LOG_FORMAT="plain"           # plain|json
DEFAULT_LOG_DIR="/var/log"
ENABLE_DEBUG=false
```

**Benefits:**
- ✅ **No code duplication** - Single logging implementation
- ✅ **Easy maintenance** - Update logging in one place
- ✅ **Consistent debugging** - Same format everywhere
- ✅ **Performance tracking** - Built-in metrics support

### **Intelligent Features**

- **Smart throttling**: Prevents notification spam with configurable intervals (5min-4hrs)
- **Auto-recovery**: Restarts failed containers and network interfaces
- **Context-aware alerts**: Different priorities and throttle times per issue type
- **Log rotation**: Automatic cleanup prevents disk space issues
- **Hash-based resumption**: Efficiently processes only new failures

## 🌐 Tailscale VPN Configuration

### **Current Setup**
- **Main System**: web1 (192.168.1.22)
- **IP**: 100.80.189.88
- **Public URL**: https://web1.tail586076.ts.net:8443/
- **Local HTTPS**: https://100.80.189.88:8443/

### **Native Installation**
- **Service**: Native systemd services (not containerized)
- **Services**: tailscaled, tailscale-serve-web1, tailscale-funnel-web1
- **Benefits**: Better performance, native OS integration

### **Tailscale systemd Services**

**tailscale-serve-web1.service:**
```ini
[Unit]
Description=Tailscale Serve HTTPS for web1 (port 8443)
After=network.target docker.service tailscaled.service
Requires=tailscaled.service

[Service]
Type=simple
ExecStartPre=/bin/sleep 30
ExecStart=/usr/bin/tailscale serve --bg --https=8443 http://localhost:8123
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**tailscale-funnel-web1.service:**
```ini
[Unit]
Description=Tailscale Funnel for web1 (public HTTPS)
After=network.target docker.service tailscaled.service
Requires=tailscaled.service

[Service]
ExecStart=/usr/bin/tailscale funnel 8443
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

### **Restore Tailscale (if needed)**
```bash
cd tailscale_native/
sudo ./restore-tailscale.sh
```

## 🐳 Docker Infrastructure

### **Core Services Stack**

```yaml
services:
  web1:
    image: ghcr.io/home-assistant/home-assistant:stable
    network_mode: host
    ports: 8123
    volumes: ./web1:/config
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "7"

  nodered:
    image: nodered/node-red:latest
    ports: 1880
    volumes: ./nodered:/data
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "7"
```

### **Docker Logging Configuration**

Global settings (`/etc/docker/daemon.json`):

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "7"
  }
}
```

**Log Management Strategy:**
- **Per Container Limit**: 70MB maximum (10MB × 7 files)
- **Total Docker Logs**: ~140MB maximum (WEB1 + NodeRED)
- **Automatic Rotation**: When log file reaches 10MB
- **Archive Policy**: Keep 7 historical log files
- **Benefits**: Prevents disk space exhaustion, maintains debugging capability

## 🏠 web1 Configuration

### **Core Configuration (configuration.yaml)**

```yaml
default_config:

http:
  use_x_forwarded_for: true
  server_host: 0.0.0.0
  trusted_proxies:
    - 127.0.0.1
    - 100.64.0.0/10

frontend:
  themes: !include_dir_merge_named themes

automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml
```

### **Text-to-Speech (TTS)**
Google Translate is used for TTS functionality:

```yaml
tts:
  - platform: google_translate
    service_name: google_say
```

### **Installed Integrations**
- ✅ **HACS** (web1 Community Store) - installed manually
- ✅ **Sonoff (eWeLink)** - 48 devices via LAN/Cloud connectivity
- ✅ **Broadlink** - functional IR/RF transmitter
- ✅ **Roomba** - auto-discovered vacuum robot
- ✅ **Weather, Sun, TTS, Backup** - built-in integrations
- ⏳ **HomeBridge** - planned for Siri integration

## 🌀 Node-RED Integration

### **Connection**
- Connected to web1 via WebSocket with long-lived token
- UI accessible at: http://192.168.1.22:1880/ (local) or via Tailscale VPN

### **Installed Palettes**
- `node-red` (core)
- `node-red-contrib-home-assistant-websocket` - WEB1 integration
- `node-red-contrib-influxdb` - time series database
- `node-red-contrib-moment` - date/time handling
- `node-red-contrib-time-range-switch` - time-based switching
- `node-red-dashboard` - web dashboard
- `node-red-node-email` - email notifications
- `node-red-node-telegrambot` - Telegram integration
- `node-red-node-ui-table` - table UI components

### **Example Automation Flow**

```json
[
  {
    "id": "sensor1",
    "type": "server-state-changed",
    "name": "Sonoff Light",
    "entityidfilter": "switch.sonoff_1000cbf589",
    "outputinitially": false,
    "x": 150,
    "y": 100,
    "wires": [["telegram"]]
  },
  {
    "id": "telegram",
    "type": "telegram sender",
    "name": "Notify",
    "bot": "mybot",
    "chatId": "538317310",
    "x": 350,
    "y": 100,
    "wires": []
  }
]
```

## 🔐 SSH Configuration

### **Quick Connection Setup**

Add to your `~/.ssh/config`:
```
# Main web1 System (Local Network)
Host web1
    HostName 192.168.1.22
    Port 22
    User user
    IdentityFile ~/.ssh/id_ed25519
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
    ServerAliveInterval 60
    ServerAliveCountMax 3

# Main System via VPN (Tailscale)
Host web1-vpn
    HostName 100.80.189.88
    Port 22
    User user
    IdentityFile ~/.ssh/id_ed25519
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
```

### **Setup SSH Key**

**Note:** SSH keys are stored in `credentials.txt` for security.

```bash
# Create the private key (get key content from credentials.txt)
nano ~/.ssh/id_ed25519
# Paste the private key from credentials.txt

# Set correct permissions
chmod 600 ~/.ssh/id_ed25519
```

### **Usage Examples**
```bash
# Local network connection
ssh web1

# VPN connection (global access)
ssh web1-vpn

# File copy operations
scp ./services/install.sh web1:/tmp/
scp -r ./services/ web1:/opt/web1/

# Remote command execution
ssh web1 "docker ps"
ssh web1-vpn "sysdiag --quick"
ssh web1-vpn systemctl status web1-watchdog
```

## 🛡️ Security Components

### **Installed Security Tools**

The system includes comprehensive security protection:

**🔥 UFW Firewall**
- **Status**: Active and enabled on system startup
- **Default Policy**: Deny incoming, allow outgoing
- **Allowed Access**:
  - SSH (22): Local network (192.168.1.0/24) + Tailscale VPN (100.64.0.0/10)
  - web1 (8123): Local network + Tailscale VPN only
  - Node-RED (1880): Local network + Tailscale VPN only
- **Blocked**: All internet access to services
- **Configuration**: `/etc/ufw/user.rules`

**🚫 Fail2ban**
- **Service**: `fail2ban.service` - active protection
- **SSH Protection**: Monitors `/var/log/auth.log`
- **Policy**: 3 failed attempts = 1 hour IP ban
- **Configuration**: `/etc/fail2ban/jail.local`
- **Status Check**: `fail2ban-client status sshd`
- **Log Rotation**: Daily rotation, 52 weeks retention (`/etc/logrotate.d/fail2ban`)

**🚨 Fail2ban Telegram Security Alerts**
- **Real-time Notifications**: Instant Telegram alerts when IPs are banned/unbanned
- **IP Geolocation**: Automatic location detection (country, city, ISP) for security threats
- **Smart Integration**: Direct fail2ban action integration - no external monitoring needed
- **Security Topic**: All alerts sent to Telegram topic 471 (Security)
- **Reboot Window Suppression**: Start/stop alerts are suppressed during nightly reboot window (03:20-03:40) to prevent noise
- **Alert Types**:
  - 🔒 IP Ban notifications with geolocation and ISP info
  - ✅ IP Unban confirmations
  - 🛡️ Fail2ban service start/stop alerts (outside reboot window)
- **Configuration**: `/etc/fail2ban/action.d/telegram-notify.conf`
- **Script**: `/usr/local/bin/telegram-fail2ban-notify.sh`
- **Logging**: `/var/log/fail2ban-telegram-notify.log`

**Example Security Alert:**
```
🚨 SECURITY BREACH DETECTED 🚨

🔒 IP BANNED: 1.2.3.4
🏛️ Service: sshd
📍 Location: Russia, Moscow
🌐 ISP: Evil Hacker ISP
⏰ Time: 2025-10-05 14:30:15

🛡️ Automatic protection activated!
⏱️ Ban duration: 1 hour
```

**📊 stress-ng**
- Performance testing utility for comprehensive system diagnostics
- Tests CPU, memory, disk I/O under load
- Integrated into health check for automated performance validation

**🌡️ Temperature Monitoring**
- Normal: < 70°C (optimized thresholds)
- High: 70-75°C (warning level)
- Critical: > 75°C (requires attention)

### **Security Configuration**

```bash
# View firewall status
ssh web1 "sudo ufw status"

# Check fail2ban status
ssh web1 "sudo fail2ban-client status"

# Run performance stress test
ssh web1 "stress-ng --cpu 1 --vm 1 --vm-bytes 100M -t 30s"
```

### **Access Control**
- **Local Network:** Full access (192.168.1.0/24)
- **Tailscale VPN:** Full access (100.64.0.0/10)
- **Internet:** Blocked by UFW firewall
- **SSH:** Key-based authentication only, passwords disabled

### **Automated Setup**

The health check system is automatically configured during installation:
- **Main script**: `/usr/local/bin/system-diagnostic.sh`
- **Quick access**: `sysdiag`, `diag`, `diagnostic`, `syscheck`, `fullcheck` commands
- **Reports**: Saved to `/tmp/system_diagnostic_YYYYMMDD_HHMMSS.txt`
- **Logs**: Diagnostic logs created automatically during execution

### **Key Setup (if needed)**
```bash
# Generate key
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519

# Copy to main system
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@192.168.1.22
```

## 🚀 Installation & Setup

### **Prerequisites**
The monitoring system requires these additional packages on the main system:
```bash
# Essential packages for monitoring functionality
sudo apt update
sudo apt install -y bc wireless-tools dos2unix curl htop git
```

### **Package Dependencies:**
- **bc**: Calculator for mathematical operations in monitoring scripts
- **wireless-tools**: WiFi signal strength monitoring (iwconfig command)
- **dos2unix**: Convert Windows line endings in configuration files
- **curl**: HTTP requests for Telegram notifications and API calls
- **htop**: Enhanced system process monitor for diagnostics and troubleshooting

### **1. Install Docker and Core Services**

**Install Docker on main system:**
```bash
# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add current user to docker group
sudo usermod -aG docker $USER

# Install Docker Compose
sudo apt update
sudo apt install docker-compose-plugin

# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Verify installation
docker --version
docker compose version
```

**Deploy web1 and Node-RED containers:**
```bash
# Create project directory
mkdir -p ~/web1
cd ~/web1

# Create docker-compose.yml file
cat > docker-compose.yml << 'EOF'
services:
  web1:
    image: ghcr.io/home-assistant/home-assistant:stable
    container_name: web1
    privileged: true
    restart: unless-stopped
    environment:
      - TZ=Europe/London
    volumes:
      - ./web1:/config
      - /run/dbus:/run/dbus:ro
    network_mode: host
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "7"

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: unless-stopped
    environment:
      - TZ=Europe/London
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "7"
EOF

# Configure Docker daemon with global logging limits
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json << 'EOF'
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "7"
  }
}
EOF

# Restart Docker to apply configuration
sudo systemctl restart docker

# Start the containers
docker compose up -d

# Verify containers are running
docker ps
```

**Access services:**
- **web1**: http://192.168.1.21:8123
- **Node-RED**: http://192.168.1.21:1880

### **2. Deploy Monitoring System**

The `install.sh` script automatically installs and configures all monitoring services, systemd units, and configurations:

```bash
# Clone or copy the web1 project to the system
git clone https://github.com/NeoSelcev/web1.git
cd web1/services

# Run the automated installation script
sudo ./install.sh
```

**What install.sh does:**

**🔧 System Preparation:**
- Checks for root privileges
- Verifies Docker installation (installs if missing)
- Installs required packages: `bc`, `wireless-tools`, `dos2unix`, `curl`, `htop`
- Adds diagnostic aliases to user shell profile

**📋 Service Installation:**
- **System Diagnostic**: `/usr/local/bin/system-diagnostic.sh` with aliases (`sysdiag`, `diag`, `diagnostic`, `syscheck`, `fullcheck`)
- **Telegram Sender**: `/usr/local/bin/telegram-sender.sh` with config template in `/etc/telegram-sender/`
- **WEB1 Watchdog**: Service + timer for 20-point health monitoring every 2 minutes
- **WEB1 Failure Notifier**: Service + timer for smart alerts and recovery every 5 minutes
- **Nightly Reboot**: Service + timer for daily maintenance reboot at 03:30
- **Update Checker**: Service + timer for weekday update analysis at 09:00
- **Backup System**: Service + timer for automated web1 backups
- **Logging Service**: Centralized log management and cleanup
- **System Diagnostic Startup**: Boot-time diagnostics

**🗂️ File Locations After Installation:**

**Scripts & Binaries:**
```
/usr/local/bin/
├── system-diagnostic.sh          # 79-check comprehensive diagnostics
├── telegram-sender.sh            # Centralized Telegram service
└── web1-monitoring-control         # Management utility
```

**Systemd Services & Timers:**
```
/etc/systemd/system/
├── web1-watchdog.service           # Health monitoring service
├── web1-watchdog.timer             # Every 2 minutes
├── web1-failure-notifier.service   # Alert & recovery service
├── web1-failure-notifier.timer     # Every 5 minutes
├── nightly-reboot.service        # Maintenance reboot
├── nightly-reboot.timer          # Daily at 03:30
├── update-checker.service        # Update analysis
├── update-checker.timer          # Weekdays 09:00
├── web1-backup.service             # Backup system
├── web1-backup.timer               # Configurable schedule
├── logging-service.service       # Log management
├── system-diagnostic-startup.service # Boot diagnostics
└── system-diagnostic-startup.timer   # At boot + 2 minutes
```

**Configuration Files:**
```
/etc/
├── telegram-sender/
│   └── config                    # Telegram bot configuration
├── web1-watchdog/
│   └── config                    # Watchdog configuration
└── logging-service/
    └── config                    # Log management configuration
```

**Log Rotation Configs:**
```
/etc/logrotate.d/
├── web1-monitoring                 # All monitoring services
├── telegram-sender               # Telegram service logs
├── web1                 # web1 logs
├── fail2ban                      # Security logs
└── ufw                          # Firewall logs
```

**State & Log Files:**
```
/var/log/
├── web1-watchdog.log              # Health monitoring logs
├── web1-failure-notifier.log      # Alert service logs
├── web1-failures.log              # Detected failures
├── web1-reboot.log                # Reboot service logs
├── web1-update-checker.log        # Update analysis logs
├── web1-backup.log                # Backup operation logs
└── telegram-sender.log          # Telegram sending logs

/var/lib/web1-failure-notifier/    # State files for smart processing
├── last_timestamp.txt           # Last processed event timestamp
├── smart_throttle_history.txt   # Throttling history
└── metadata.txt                 # File rotation detection
```

**🚀 Service Startup:**
After installation, all services are automatically:
- **Enabled**: Start automatically on boot
- **Started**: Begin monitoring immediately
- **Configured**: Ready with default settings
- **Logged**: All activities are logged with rotation

**⚙️ Management Commands:**
```bash
# Check installation status
sudo web1-monitoring-control status

# Start all monitoring services
sudo web1-monitoring-control start

# View recent logs
sudo web1-monitoring-control logs

# Test Telegram integration
sudo web1-monitoring-control test-telegram
```

### **3. Configure Telegram Bot**

**Check telegram updates on https://api.telegram.org/bot8185210583:AAG8wijjUfAFHTyP-rzI1WpVyxcJEJQAIXQ/getUpdates**

Create centralized telegram-sender configuration:

1. Create a bot via @BotFather in Telegram
2. Obtain the bot token and group ID with discussion topics
3. Create the telegram-sender configuration:

```bash
sudo mkdir -p /etc/telegram-sender
sudo tee /etc/telegram-sender/config << 'EOF'
# Core bot settings
TELEGRAM_BOT_TOKEN="your_bot_token_here"
TELEGRAM_CHAT_ID="your_group_chat_id"

# Group topics (message_thread_id)
TELEGRAM_TOPIC_SYSTEM=2             # System messages
TELEGRAM_TOPIC_ERRORS=10            # Errors and failures
TELEGRAM_TOPIC_UPDATES=9            # Updates
TELEGRAM_TOPIC_RESTART=4            # Restarts
TELEGRAM_TOPIC_SYSTEM_DIAGNOSTIC=123  # System diagnostic reports
TELEGRAM_TOPIC_BACKUP=131           # Backup reports

# Performance settings
TELEGRAM_TIMEOUT=10
TELEGRAM_RETRY_COUNT=3
TELEGRAM_RETRY_DELAY=2
EOF
```

### **4. Management Commands**
```bash
# Start monitoring
sudo web1-monitoring-control start

# Check status
sudo web1-monitoring-control status

# View logs
sudo web1-monitoring-control logs

# Test Telegram
sudo web1-monitoring-control test-telegram

# Stop monitoring
sudo web1-monitoring-control stop
```

## 📊 Monitoring Dashboard

### **Service Status Check**
```bash
# Quick system overview
ssh rpi "vcgencmd measure_temp && free -h && docker ps"

# Monitoring service status
ssh rpi "systemctl status web1-watchdog.timer web1-failure-notifier.timer"

# Recent failures
ssh rpi "tail -20 /var/log/web1-failures.log"
```

### **Log Files Location**
- **Watchdog**: `/var/log/web1-watchdog.log`
- **Failure Notifier**: `/var/log/web1-failure-notifier.log`
- **Failures**: `/var/log/web1-failures.log`
- **Reboot**: `/var/log/web1-reboot.log`
- **Updates**: `/var/log/web1-update-checker.log`
- **Telegram Sender**: `/var/log/telegram-sender.log`

## 🔧 System Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   web1-watchdog   │───▶│  /var/log/       │───▶│ web1-failure-     │
│   (2 minutes)   │    │  web1-failures.log │    │ notifier        │
│                 │    │                  │    │ (5 minutes)     │
│ • 20 health     │    │ • Failure events │    │                 │
│   checks        │    │ • Timestamps     │    │ • Telegram      │
│ • Auto recovery │    │ • Error details  │    │   alerts        │
│ • Logging       │    │                  │    │ • Throttling    │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                         │
                                                         ▼
                                               ┌─────────────────┐
                                               │   Telegram      │
                                               │   Bot           │
                                               │                 │
                                               │ 🚨 Critical     │
                                               │ ⚠️  Warning     │
                                               │ ℹ️  Info        │
                                               │ 📊 Status       │
                                               └─────────────────┘
```

## 🔧 Troubleshooting & System Health

### **Common Diagnostics**

```bash
# System health overview
ssh rpi "vcgencmd measure_temp && free -h && df -h"

# Enhanced system monitoring with htop
ssh rpi "htop -d 10 -n 1"

# Full system diagnostics (79 checks)
ssh rpi "sysdiag"

# Quick system check
ssh rpi "syscheck"

# Service status check
ssh rpi "systemctl status web1-watchdog.timer web1-failure-notifier.timer"

# Docker container health
ssh rpi "docker ps && docker stats --no-stream"

# Recent failure events
ssh rpi "tail -20 /var/log/web1-failures.log"

# Network connectivity test
ssh rpi "ping -c 3 8.8.8.8 && curl -s https://www.google.com"
```

### **Debugging Commands**

**Check Tailscale status:**
```bash
tailscale ip -4
tailscale status
```

**web1 status:**
```bash
docker logs -f web1
curl -v http://localhost:8123
```

**Network diagnostics:**
```bash
# Check network connectivity
ping 8.8.8.8
systemctl status networking

# WiFi signal strength
iwconfig wlan0

# Port availability
ss -tulpn | grep :8123
```

**System performance:**
```bash
# System resources
htop
free -h
df -h

# Temperature monitoring
vcgencmd measure_temp
```

# Temperature monitoring
vcgencmd measure_temp
```

### **Performance Optimizations**

- **Reduced I/O**: Watchdog runs every 2min instead of 15s (8x less frequent)
- **Smart dependencies**: Services start only when prerequisites are ready
- **Efficient logging**: Proper log rotation prevents disk space issues
- **Load balancing**: Randomized delays prevent system load spikes
- **Enhanced monitoring**: Expanded from 17 to 20 comprehensive health checks
- **Intelligent throttling**: 60-minute notification throttling prevents spam

### **🎉 Deployment Status: FULLY OPERATIONAL**

✅ **All 4 monitoring services active and scheduled:**
- web1-watchdog.timer (every 2 minutes)
- web1-failure-notifier.timer (every 5 minutes)
- nightly-reboot.timer (daily at 3:30 AM)
- update-checker.timer (weekdays at 9:00 AM ±30min)

✅ **System packages installed:** bc, wireless-tools, dos2unix, curl
✅ **Telegram integration:** Active and sending notifications
✅ **Auto-recovery:** Container and network interface restart capabilities
✅ **Boot persistence:** All services enabled for automatic startup

## � Monitoring and Diagnostics

### **Key Metrics**
- **CPU temperature** (normal <65°C, critical >70°C)
- **System load** (CPU, RAM, disk)
- **Service availability** (ping, port check)
- **Docker container status**

### **Automatic Recovery**
- **Restart failed containers**
- **Restore WiFi interface**
- **Clean logs when disk fills**
- **Notify about all actions**

## �🖥️ Hardware & OS Configuration

### **Primary Node Configuration**
- **OS**: Debian 12 (Bookworm) ARM64
- **IP Address**: 192.168.1.22 (static)
- **Hostname**: web1
- **Storage**: 16GB MicroSD Card (SanDisk Ultra)

### **Network Configuration**
- **Main network**: 192.168.1.0/24
- **IoT subnets**: 192.168.2.x, 192.168.3.x, 192.168.4.x
- **DNS**: 8.8.8.8, 1.1.1.1
- **Ports**:
  - **8123**: web1 Web UI
  - **1880**: Node-RED Flow Editor
  - **22**: SSH Management Port
  - **443/80**: HTTPS/HTTP (Tailscale Funnel)

## 🌐 Home Network Infrastructure

### Core Devices
- **Main router**: Technicolor FGA2233PTN (fiber)
- **Mesh system**: TP-Link Deco HC220-G1-IL (coverage extension)
- **IoT routers**: Isolated networks for smart devices
- **WiFi extender**: TP-Link RE305

### Configuration
- **Network**: 192.168.1.0/24
- **Pi address**: 192.168.1.21 (static)
- **DNS**: 8.8.8.8, 1.1.1.1
- **VPN**: Tailscale for remote access

## 📁 Project Structure

```
PRI-WEB1/
├── 📋 README.md                           # This comprehensive documentation
├── 🐳 docker/                            # Docker infrastructure
│   ├── docker-compose.yml                # Docker stack (WEB1 + Node-RED)
│   └── daemon.json                       # Docker daemon configuration
├── 📁 services/                          # Complete monitoring system
│   ├── install.sh                        # Automated installation script
│   ├── communication/                    # Communication services
│   │   └── telegram-sender/              # Centralized Telegram service v1.0
│   │       ├── telegram-sender.sh        # Main script
│   │       ├── telegram-sender.conf      # Configuration
│   │       └── telegram-sender.logrotate # Log rotation
│   ├── diagnostics/                      # System diagnostics
│   │   ├── system-diagnostic.sh          # 79-check comprehensive diagnostics
│   │   └── system-diagnostic.logrotate   # Log rotation config
│   ├── monitoring/                       # Health monitoring services
│   │   ├── web1-watchdog/                  # 20-point system monitoring (every 2min)
│   │   │   ├── web1-watchdog.sh            # Main monitoring script
│   │   │   ├── web1-watchdog.conf          # Configuration
│   │   │   ├── web1-watchdog.service       # Systemd service
│   │   │   ├── web1-watchdog.timer         # Systemd timer
│   │   │   └── web1-watchdog.logrotate     # Log rotation
│   │   └── web1-failure-notifier/          # Smart alerts & recovery (every 5min)
│   │       ├── web1-failure-notifier.sh    # Notification script
│   │       ├── web1-failure-notifier.service # Systemd service
│   │       ├── web1-failure-notifier.timer # Systemd timer
│   │       └── web1-failure-notifier.logrotate # Log rotation
│   ├── system/                           # System maintenance services
│   │   ├── nightly-reboot/               # Daily maintenance reboot (03:30)
│   │   │   ├── nightly-reboot.sh         # Reboot script
│   │   │   ├── nightly-reboot.service    # Systemd service
│   │   │   ├── nightly-reboot.timer      # Systemd timer
│   │   │   └── nightly-reboot.logrotate  # Log rotation
│   │   ├── update-checker/               # Update analysis (weekdays 09:00)
│   │   │   ├── update-checker.sh         # Update checking script
│   │   │   ├── update-checker.service    # Systemd service
│   │   │   ├── update-checker.timer      # Systemd timer
│   │   │   └── update-checker.logrotate  # Log rotation
│   │   ├── web1-backup/                    # Backup system
│   │   │   ├── web1-backup.sh              # Backup script
│   │   │   ├── web1-restore.sh             # Restore script
│   │   │   ├── web1-backup.service         # Systemd service
│   │   │   ├── web1-backup.timer           # Systemd timer
│   │   │   └── web1-backup.logrotate       # Log rotation
│   │   ├── logging-service/              # Centralized log management
│   │   │   ├── logging-service.sh        # Log management script
│   │   │   ├── logging-service.conf      # Configuration
│   │   │   ├── logging-service.service   # Systemd service
│   │   │   └── logging-service.logrotate # Log rotation
│   │   ├── system-diagnostic-startup/    # Startup diagnostics
│   │   │   ├── system-diagnostic-startup.sh # Startup script
│   │   │   ├── system-diagnostic-startup.service # Systemd service
│   │   │   ├── system-diagnostic-startup.timer # Systemd timer
│   │   │   └── system-diagnostic-startup.logrotate # Log rotation
│   │   └── web1-general-logs.logrotate     # General log rotation config
│   ├── logrotate/                        # System log rotation configs
│   │   ├── web1                 # WEB1 log rotation
│   │   ├── fail2ban                      # Security log rotation
│   │   ├── ufw                           # Firewall log rotation
│   │   └── journald.conf                 # Systemd journal limits
│   └── tailscale/                        # Tailscale VPN services
│       ├── scripts/                      # Utility scripts
│       │   └── remote-delete-machines    # Machine cleanup script
│       ├── tailscaled/                   # Native daemon service
│       │   ├── tailscaled.service        # Systemd service
│       │   └── tailscaled.default        # Environment config
│       ├── tailscale-serve-web1/           # HTTPS proxy service
│       │   └── tailscale-serve-web1.service # Systemd service
│       └── tailscale-funnel-web1/          # Public HTTPS access
│           └── tailscale-funnel-web1.service # Systemd service
└── 📁 docs/                              # Documentation & architecture
    ├── network-infrastructure.md         # Network topology
    └── images/                           # Network diagrams & photos
        ├── Home plan.jpg                 # House layout
        ├── Home plan - routers.jpg       # Router placement
        └── Home plan - smart devices.JPEG # Device locations
```

## 🔍 Docker Audit Logging

Advanced Docker security monitoring with system-level auditing to track container operations, configuration changes, and security events.

### Purpose

Monitor all Docker daemon activities including:
- Container lifecycle events (create, start, stop, delete)
- Image operations (pull, push, delete)
- Volume and network modifications
- Docker daemon configuration changes
- Security-sensitive operations (privileged containers, capability additions)

### Implementation

**Audit Rules Location:**
```bash
/etc/audit/rules.d/docker.rules
```

**Key Monitoring Points:**
- Docker socket: `/var/run/docker.sock`
- Docker daemon binary: `/usr/bin/dockerd`
- Docker configuration: `/etc/docker/daemon.json`
- Container runtime: `/usr/bin/containerd`
- Docker Compose files: `/opt/web1/docker-compose*.yml`

**Example Audit Rules:**
```bash
# Docker daemon execution
-w /usr/bin/dockerd -p x -k docker_daemon

# Docker socket access
-w /var/run/docker.sock -p rwxa -k docker_socket

# Container runtime
-w /usr/bin/containerd -p x -k container_runtime
-w /usr/bin/runc -p x -k container_runtime

# Docker configuration changes
-w /etc/docker/ -p wa -k docker_config
-w /etc/default/docker -p wa -k docker_config

# Docker Compose files
-w /opt/web1/docker-compose.yml -p wa -k docker_compose
-w /opt/web1/docker-compose-with-tunnel.yml -p wa -k docker_compose

# Container systemd service
-w /etc/systemd/system/docker.service.d/ -p wa -k docker_systemd
```

### Viewing Audit Logs

```bash
# All Docker-related events
ausearch -k docker_daemon -k docker_socket -k container_runtime -k docker_config

# Recent Docker socket access
ausearch -k docker_socket -ts recent

# Docker daemon executions
ausearch -k docker_daemon -x /usr/bin/dockerd

# Configuration changes
ausearch -k docker_config -ts today

# Specific container events
ausearch -k docker_socket | grep "container_name"

# Failed Docker operations
ausearch -k docker_socket --success no
```

### Journal Retention

Configure systemd journal to retain Docker logs:

```bash
# /etc/systemd/journald.conf
[Journal]
SystemMaxUse=500M
SystemKeepFree=1G
MaxRetentionSec=1month
MaxFileSec=1week
```

**Apply configuration:**
```bash
sudo systemctl restart systemd-journald
```

### Integration with Monitoring

Docker audit events are integrated with system diagnostics:

```bash
# Check Docker audit logs
sudo ausearch -k docker_daemon -k docker_socket --start recent

# View Docker container logs
sudo docker logs web1
sudo docker logs nodered
sudo docker logs cloudflared-tunnel

# Check Docker daemon journal
sudo journalctl -u docker -n 100 --no-pager
```

### Benefits

- ✅ **Security Compliance** - Track all privileged operations
- ✅ **Incident Response** - Forensic analysis of container breaches
- ✅ **Change Tracking** - Audit configuration modifications
- ✅ **Troubleshooting** - Identify failed operations
- ✅ **Performance Analysis** - Monitor container lifecycle patterns

## 🔒 System Auditing with auditd

Linux Audit Daemon (`auditd`) provides comprehensive system-level security monitoring for critical system operations.

### Purpose

Monitor security-critical system events:
- SSH authentication attempts (successful and failed)
- systemd service changes
- Firewall rule modifications (UFW/iptables)
- File access to sensitive directories
- Privilege escalation (sudo usage)
- User and group modifications
- System configuration changes

### Implementation

**Audit Rules Location:**
```bash
/etc/audit/rules.d/audit.rules
```

**Key Monitoring Categories:**

**1. SSH Access Monitoring**
```bash
# SSH daemon
-w /usr/sbin/sshd -p x -k ssh_daemon

# SSH configuration
-w /etc/ssh/sshd_config -p wa -k ssh_config

# SSH keys
-w /home/macbookpro12-1/.ssh/ -p wa -k ssh_keys
-w /root/.ssh/ -p wa -k ssh_keys

# PAM authentication
-w /var/log/auth.log -p wa -k auth_log
```

**2. systemd Service Changes**
```bash
# systemd unit files
-w /etc/systemd/system/ -p wa -k systemd_units
-w /usr/lib/systemd/system/ -p wa -k systemd_units

# systemd control
-a always,exit -F arch=b64 -S execve -F path=/bin/systemctl -k systemd_control
```

**3. Firewall Changes**
```bash
# UFW configuration
-w /etc/ufw/ -p wa -k firewall_config
-w /usr/sbin/ufw -p x -k firewall_cmd

# iptables direct access
-w /usr/sbin/iptables -p x -k firewall_iptables
-w /usr/sbin/ip6tables -p x -k firewall_iptables

# Netfilter configuration
-a always,exit -F arch=b64 -S setsockopt -F a0=41 -k netfilter_config
```

**4. Privilege Escalation**
```bash
# sudo usage
-w /usr/bin/sudo -p x -k sudo_usage
-w /etc/sudoers -p wa -k sudo_config
-w /etc/sudoers.d/ -p wa -k sudo_config

# su command
-w /usr/bin/su -p x -k privilege_escalation
```

**5. User and Group Changes**
```bash
# Password files
-w /etc/passwd -p wa -k user_modification
-w /etc/shadow -p wa -k user_modification
-w /etc/group -p wa -k group_modification

# User management commands
-w /usr/sbin/useradd -p x -k user_management
-w /usr/sbin/userdel -p x -k user_management
-w /usr/sbin/usermod -p x -k user_management
-w /usr/sbin/groupadd -p x -k group_management
-w /usr/sbin/groupdel -p x -k group_management
```

**6. Monitoring Service Files**
```bash
# web1 monitoring scripts
-w /usr/local/bin/web1-watchdog.sh -p wa -k monitoring_scripts
-w /usr/local/bin/web1-failure-notifier.sh -p wa -k monitoring_scripts
-w /usr/local/bin/system-diagnostic.sh -p wa -k monitoring_scripts
-w /usr/local/bin/web1-monitoring-control -p wa -k monitoring_scripts

# Configuration files
-w /etc/web1-watchdog.conf -p wa -k monitoring_config
-w /etc/telegram-sender/ -p wa -k telegram_config
```

### Viewing Audit Logs

```bash
# SSH access attempts
ausearch -k ssh_daemon -k ssh_config -ts today

# Failed SSH logins
ausearch -k ssh_daemon --success no

# systemd service changes
ausearch -k systemd_units -k systemd_control -ts recent

# Firewall modifications
ausearch -k firewall_config -k firewall_cmd -ts today

# sudo usage
ausearch -k sudo_usage -ts today

# User modifications
ausearch -k user_modification -k group_modification

# All security events today
ausearch -ts today | grep -E "ssh|sudo|systemd|firewall|user"

# Generate audit report
aureport --summary

# Failed authentication attempts
aureport --auth --failed

# Command execution report
aureport -x --summary
```

### Audit Search Examples

```bash
# Who accessed SSH configuration?
ausearch -k ssh_config -i

# What systemd services were modified?
ausearch -k systemd_units -ts this-week -i

# Firewall rule changes in last 24 hours
ausearch -k firewall_config -ts recent -i

# All sudo commands by specific user
ausearch -k sudo_usage -ui 1000

# Failed privilege escalation attempts
ausearch -k sudo_usage --success no -ts today

# Monitoring script modifications
ausearch -k monitoring_scripts -k monitoring_config
```

### Integration with System Diagnostics

Audit logs are checked by `system-diagnostic.sh`:

```bash
# Run diagnostics with audit check
sysdiag

# Check specific audit categories
sudo ausearch -k ssh_daemon -ts recent
sudo ausearch -k firewall_config -ts today
sudo ausearch -k systemd_units -ts recent
```

### Audit Log Retention

Configure audit log retention:

```bash
# /etc/audit/auditd.conf
max_log_file = 10
num_logs = 10
max_log_file_action = ROTATE
space_left = 100
space_left_action = SYSLOG
admin_space_left = 50
admin_space_left_action = SUSPEND
```

**Total storage:** ~100MB (10 files × 10MB each)

### Benefits

- ✅ **Security Monitoring** - Track all security-sensitive operations
- ✅ **Compliance** - Meet security audit requirements
- ✅ **Forensics** - Investigate security incidents
- ✅ **Change Tracking** - Audit all system modifications
- ✅ **Intrusion Detection** - Detect unauthorized access attempts
- ✅ **Accountability** - Track user actions and system changes

### Audit Best Practices

1. **Regular Review** - Check audit logs weekly
2. **Automated Alerts** - Configure auditd to alert on critical events
3. **Log Retention** - Keep at least 30 days of audit logs
4. **Secure Storage** - Protect audit logs from tampering
5. **Performance Impact** - Monitor system performance with auditing enabled
6. **Rule Optimization** - Only audit security-relevant events

## ⚠️ Known Issues

- **Telegram YAML configuration is deprecated** - migrate to UI integration for better reliability
- **SSL error when using Funnel without certificate** - certificate auto-renewal may fail
- **WEB1 Mobile may occasionally lose VPN connectivity** - restart Tailscale service on mobile device
- **Large log files** - ensure logrotate is running properly via `systemctl status logrotate.timer`
- **Memory pressure on Pi 3B+** - monitor swap usage and consider log cleanup if system becomes slow

## 💡 Recommendations and ToDo

### **Security Enhancements**
- 🔐 Enable authentication and role management in web1
- 🔑 Implement regular SSH key rotation
- 🛡️ Consider enabling two-factor authentication for critical services

### **Integration Expansion**
- 🧩 Configure HomeBridge for Siri integration
- 🌍 Use Tailscale DNS or custom domain via CNAME
- 🧪 Add integrations: Zigbee2MQTT (USB), ESPHome, MQTT broker
- 📡 Expand Telegram notifications (motion, temperature, events)

### **Backup & Maintenance**
- 🔄 Implement automated snapshot scheduling
- 📲 Automate backups to external disk or Google Drive
- 📊 Set up InfluxDB for historical data retention
- 🧹 Configure automated disk cleanup routines

### **Smart Home Automation**
- 🧠 Build structured Node-RED automations:
  - Motion-based lighting control
  - Night mode activation
  - Security deterrence systems
  - Environmental monitoring alerts

---
*Smart Home Monitoring System - Comprehensive health monitoring with intelligent alerting for web1 installations.*
