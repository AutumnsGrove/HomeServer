# Le Potato Home Server - Implementation TODOs

**Last Updated:** October 22, 2025
**Project Status:** Planning → Implementation (Awaiting Hardware)
**Current Phase:** Phase 0 - Hardware Acquisition

---

## Phase 0: Hardware Acquisition 🛒

### Essential Hardware (Required)
- [ ] Order eMMC module (32GB or 64GB) - $25-45
- [ ] Order USB eMMC writer - $10-15
- [ ] Order heatsink + 3010 fan kit - $10-15
- [ ] Order powered USB 3.0 hub (4+ ports, 2A+ per port) - $25-35
- [ ] Order 5V 4A power supply for Le Potato - $10-15
- [ ] Verify external SSD availability (USB 3.0, 1-2TB)

### Recommended Hardware (Optional but Beneficial)
- [ ] Second external SSD for backups - $70-120
- [ ] USB power meter for diagnostics - $15-20
- [ ] Thermal paste (if not included with heatsink) - $5-10

**Phase 0 Complete When:** All essential hardware ordered and arrival dates confirmed

---

## Phase 1: Base System Setup ⚙️

### eMMC Preparation
- [ ] Download Ubuntu 24.04 LTS Server ARM64 image
- [ ] Write Ubuntu image to eMMC using USB writer
- [ ] Test eMMC boot (verify system recognizes eMMC)
- [ ] Configure initial user account and SSH access

### Cooling Installation
- [ ] Install heatsink on SoC (apply thermal paste if needed)
- [ ] Mount 3010 fan with proper airflow direction
- [ ] Test fan operation and verify temperature drops
- [ ] Set fan to run continuously (or configure PWM if available)

### Base OS Configuration
- [ ] Set static IP address or DHCP reservation
- [ ] Configure SSH key-based authentication
- [ ] Disable password SSH login (security hardening)
- [ ] Update system packages: `apt update && apt upgrade`
- [ ] Configure timezone and locale settings
- [ ] Set hostname to something memorable (e.g., `lepotato-server`)

### Thermal Monitoring Setup
- [ ] Install `lm-sensors` and configure
- [ ] Verify CPU temperature monitoring works
- [ ] Document baseline temps (idle and under load)
- [ ] Confirm temps stay <55°C under load

**Phase 1 Complete When:** System boots from eMMC, SSH accessible, temps monitored and acceptable

---

## Phase 2: Storage Configuration 💾

### USB Hub and SSD Setup
- [ ] Connect powered USB hub to Le Potato
- [ ] Connect external SSD to powered hub
- [ ] Verify SSD is recognized: `lsblk`
- [ ] Check USB power draw with power meter (if available)

### Filesystem and Mounting
- [ ] Partition SSD (single ext4 partition recommended)
- [ ] Format partition: `mkfs.ext4 -L data /dev/sdX1`
- [ ] Create mount point: `/mnt/data`
- [ ] Configure /etc/fstab for automatic mounting
- [ ] Test mount persistence across reboots
- [ ] Set proper permissions on /mnt/data

### Docker Installation
- [ ] Install Docker Engine (ARM64 version)
- [ ] Add user to docker group
- [ ] Configure Docker to use overlay2 storage driver
- [ ] Set Docker data directory to /mnt/data/docker
- [ ] Configure Docker daemon.json with resource limits
- [ ] Enable Docker service: `systemctl enable docker`
- [ ] Verify Docker working: `docker run hello-world`

### /var/log Relocation (Optional but Recommended)
- [ ] Create /mnt/data/var-log directory
- [ ] Configure rsyslog to write to external SSD
- [ ] Symlink or bind mount /var/log if needed
- [ ] Verify log writing to SSD
- [ ] Configure logrotate for new location

**Phase 2 Complete When:** SSD mounted, Docker configured and working, logs relocated

---

## Phase 3: Core Services 🌐

### Tailscale VPN
- [ ] Sign up for Tailscale account (if not already)
- [ ] Install Tailscale on Le Potato
- [ ] Authenticate and connect to Tailnet
- [ ] Enable subnet routing (if needed for LAN access)
- [ ] Test remote SSH access via Tailscale IP
- [ ] Document Tailscale IP address

### Pi-hole (Primary Production Service)
- [ ] Pull Pi-hole Docker image (ARM64)
- [ ] Create docker-compose.yml for Pi-hole
- [ ] Configure Pi-hole with host network mode or macvlan
- [ ] Set custom DNS upstream servers
- [ ] Start Pi-hole container with restart policy
- [ ] Access Pi-hole web UI and complete setup
- [ ] Configure router to use Pi-hole as DNS (or configure devices individually)
- [ ] Test ad blocking functionality
- [ ] Set Pi-hole admin password

### Tailscale + Pi-hole Integration
- [ ] Configure Tailscale to use Pi-hole as DNS
- [ ] Test DNS resolution from remote Tailscale clients
- [ ] Verify ad blocking works on remote devices
- [ ] Document configuration for future reference

**Phase 3 Complete When:** Tailscale accessible, Pi-hole blocking ads on LAN and remotely

---

## Phase 4: Monitoring Stack 📊

### VictoriaLogs
- [ ] Pull VictoriaLogs Docker image (ARM64)
- [ ] Create docker-compose.yml for VictoriaLogs
- [ ] Configure storage path on /mnt/data
- [ ] Set memory limits (200-250MB recommended)
- [ ] Start VictoriaLogs container
- [ ] Verify VictoriaLogs API accessible

### Grafana
- [ ] Pull Grafana Docker image (ARM64)
- [ ] Create docker-compose.yml for Grafana
- [ ] Configure persistent storage for dashboards
- [ ] Set memory limits (150-200MB)
- [ ] Start Grafana container
- [ ] Access Grafana web UI
- [ ] Add VictoriaLogs as data source
- [ ] Import home server dashboard templates

### Netdata (Optional - Resource Permitting)
- [ ] Install Netdata (native or container)
- [ ] Configure Netdata to send logs to VictoriaLogs
- [ ] Set Netdata memory limits
- [ ] Access Netdata web UI
- [ ] Verify real-time monitoring working

### Alert Configuration
- [ ] Configure Grafana alerts for:
  - [ ] High CPU temperature (>60°C)
  - [ ] High RAM usage (>85%)
  - [ ] Disk space low (<15% free)
  - [ ] Pi-hole service down
  - [ ] Docker daemon issues
- [ ] Set up notification channels (email, Discord, etc.)
- [ ] Test alert firing and notifications

**Phase 4 Complete When:** All monitoring services running, dashboards accessible, alerts configured

---

## Phase 5: Backup and Reliability 💪

### Backup Solution
- [ ] Install rsync (if not already installed)
- [ ] Create backup script for Docker volumes
- [ ] Configure hard-link snapshot backups (7-day retention)
- [ ] Set up cron job for daily backups
- [ ] Test backup and restore procedures
- [ ] Document backup locations and restore process

### System Reliability
- [ ] Configure Docker restart policies on all containers
- [ ] Test container auto-restart after crash
- [ ] Test system recovery after power loss
- [ ] Verify all services start on boot
- [ ] Document recovery procedures

### Optional: Second SSD for Backups
- [ ] Connect second SSD to powered hub
- [ ] Format and mount second SSD
- [ ] Configure backup script to use second SSD
- [ ] Test cross-SSD backup functionality

**Phase 5 Complete When:** Automated backups running, system self-heals, recovery tested

---

## Phase 6: Development Environment (On-Demand) 💻

**Note:** Dev environment is mutually exclusive with monitoring stack due to RAM constraints. Stop monitoring services before starting dev services.

### Claude Code Setup
- [ ] Research Claude Code ARM compatibility (reference: docs/specs/SW-04)
- [ ] Install Claude Code if ARM-compatible
- [ ] Configure Claude Code for remote development
- [ ] Test Claude Code functionality
- [ ] Document any ARM-specific limitations

### happy.engineering Setup
- [ ] Research happy.engineering ARM support (reference: docs/specs/SW-05)
- [ ] Install happy.engineering if ARM-compatible
- [ ] Configure happy.engineering
- [ ] Test development workflow
- [ ] Document setup and usage

### Mode Switching Script
- [ ] Create script to stop monitoring services
- [ ] Create script to start dev services
- [ ] Create script to switch back to monitoring mode
- [ ] Test mode switching
- [ ] Document mode switching procedure

**Phase 6 Complete When:** Dev environment functional, mode switching reliable

---

## Future Enhancements 🚀

### Nice-to-Have Features
- [ ] Set up automated system updates
- [ ] Configure email notifications for system events
- [ ] Add UPS integration (if UPS acquired)
- [ ] Implement advanced network monitoring
- [ ] Set up Prometheus for metrics collection
- [ ] Create custom Grafana dashboards
- [ ] Implement automated health checks
- [ ] Add more services (Jellyfin, Nextcloud, etc.) - RAM permitting

### Performance Optimization
- [ ] Tune Docker resource limits based on usage
- [ ] Optimize log retention policies
- [ ] Fine-tune backup schedules
- [ ] Profile RAM usage and adjust accordingly

### Documentation
- [ ] Create troubleshooting guide for common issues
- [ ] Document all custom configurations
- [ ] Create runbook for system maintenance
- [ ] Document lessons learned

---

## Blocked/Waiting

### Waiting on Hardware
- All implementation phases blocked until Phase 0 hardware acquisition complete
- Estimated delivery: [Add dates when ordered]

---

## Notes

- **Total Estimated Time:** 4-6 weeks from hardware arrival to full deployment
- **RAM Budget:** Critical constraint - carefully monitor all services
- **Thermal Management:** Active cooling is mandatory - DO NOT skip
- **Power:** Powered USB hub is required for SSD stability
- **Backup:** Test recovery procedures regularly

---

**Project Confidence:** 🟢 HIGH (90%) - Research-validated architecture
**Ready for Implementation:** ✅ YES - Awaiting hardware only
