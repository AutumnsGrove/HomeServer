# Le Potato Home Server Project Specification

**Version:** 1.0  
**Date:** October 11, 2025  
**Target Device:** Le Potato (Raspberry Pi-like SBC)  
**Primary Goal:** Long-term, self-healing home server with remote access and monitoring

---

## Project Overview

Transform Le Potato device into a reliable, containerized home server with:
- Remote access via Tailscale VPN
- Network-wide ad blocking (Pi-hole)
- NAS capabilities with external SSD storage
- On-demand development environment
- Self-healing architecture with automatic restarts
- Comprehensive logging with web-based monitoring

---

## Core Requirements

### Reliability & Uptime
- **Self-healing:** Services automatically restart on failure
- **Scheduled maintenance:** Weekly automatic system restart
- **Boot resilience:** System recovers gracefully from power loss
- **Monitoring:** Real-time visibility into system and service health

### Access Model
- **Primary:** SSH access from Tailscale-connected devices
- **Secondary:** Web UIs accessible via Tailscale network or local network
- **No local interaction:** No need to connect monitor/keyboard to device

### Data Persistence
- Critical configurations survive restarts
- Docker volumes properly persisted
- External storage auto-mounts on boot

---

## Architecture Decisions

### Operating System
**Ubuntu Server ARM (64-bit)**
- Rationale: Excellent ARM support, mature Docker ecosystem, large community
- Version: Latest LTS release
- Installation: Fresh install (refresh from current Armbian)

### Container Strategy
**Docker + Docker Compose**
- Bare metal: Tailscale, Homebrew, uv, tmux
- Containerized: Pi-hole, development environment, file sharing services, monitoring stack
- Management: systemd integration for auto-start

### Storage Architecture
- **Boot:** microSD card (256GB SanDisk) - relatively static after setup
- **Heavy I/O:** External SSD for Docker volumes, logs, NAS data
- **Filesystem:** ext4 for simplicity (research: btrfs for snapshots?)
- **Target capacity:** 2TB external storage (planning for future)

---

## Service Breakdown

### Bare Metal Services

#### 1. Tailscale
- **Purpose:** Secure remote access, subnet routing
- **Configuration:** 
  - Enable subnet router mode
  - Set as DNS resolver for Tailscale network
  - Auto-start on boot
- **Priority:** Critical - required for remote access

#### 2. System Utilities
- **Homebrew:** Package management
- **uv:** Python package/environment manager  
- **tmux:** Terminal multiplexing for persistent sessions
- **Installation:** System-level, available to all users

### Containerized Services

#### 1. Pi-hole (Production Service)
- **Container:** Official Pi-hole Docker image
- **Purpose:** Network-wide ad/tracker blocking
- **Network:** Host network mode or macvlan (research needed)
- **Integration:** Configured as Tailscale DNS upstream
- **Data persistence:** Config volume, query logs
- **Health check:** DNS query response
- **Priority:** CRITICAL - main production service

#### 2. Development Environment Container
- **Purpose:** Remote coding workspace with Claude Code + happy.engineering
- **Startup:** On-demand (not always running)
- **Components:**
  - Claude Code CLI
  - happy.engineering
  - tmux (persistent sessions)
  - Development tools/SDKs as needed
- **Access:** SSH into host, attach to tmux session in container
- **Data persistence:** Project files, config, shell history
- **Priority:** Medium - used as needed

#### 3. File Sharing / NAS
- **Purpose:** Network file access to external SSD storage
- **Protocol:** Samba (SMB) for wide compatibility
- **Access:** Via Tailscale network or local network
- **Storage:** Mounted external SSD volumes
- **Priority:** Medium - planning for future use

#### 4. Monitoring Stack
- **Primary:** Grafana + Loki (full observability)
  - Grafana: Visualization and dashboards
  - Loki: Log aggregation from Docker containers
  - Promtail: Log shipping agent
- **Secondary:** Netdata (system metrics)
  - CPU, memory, disk, network stats
  - Real-time performance monitoring
- **Access:** Web UI via Tailscale network
- **Data retention:** 7-14 days (balance storage vs history)
- **Emphasis:** Docker container logs > bare metal logs
- **Priority:** High - required for observability

---

## Storage Strategy

### microSD Card Longevity Concerns
- **Expected lifespan:** 1-3 years under moderate server load
- **Mitigation strategies:**
  1. **Research:** Can Le Potato boot from USB? (Model-specific)
  2. Move high-write workloads to external SSD:
     - Docker volumes → external SSD
     - System logs → external SSD (symlink `/var/log`)
     - Swap → external SSD or disable
  3. Reduce writes to SD card:
     - Disable unnecessary logging
     - Use tmpfs for temporary files
     - Periodic cleanup of package caches

### External SSD Setup
- **Count:** Planning for 1-3 external SSDs
- **Connection:** USB 3.0 (verify Le Potato USB capabilities)
- **Auto-mount:** `/etc/fstab` or systemd mount units
- **Use cases:**
  - Docker volumes
  - NAS file storage  
  - System logs
  - Backups
- **File system:** ext4 (research: btrfs for snapshots/integrity?)

---

## Implementation Phases

### Phase 0: Research & Pre-flight
**Research Questions:**
1. Le Potato USB boot capability (model-specific)
2. Optimal USB storage configuration for Le Potato
3. Pi-hole Docker networking approach (host vs macvlan)
4. Grafana/Loki resource requirements on ARM
5. Best practices for SD card wear reduction on ARM devices
6. External SSD power requirements (self-powered vs bus-powered)

### Phase 1: Base System Setup
1. Back up current Armbian configuration/data
2. Flash Ubuntu Server ARM to microSD
3. Initial boot and SSH access
4. System updates and essential packages
5. User account configuration
6. SSH key authentication
7. Disable password authentication

### Phase 2: Storage Configuration
1. Connect and format external SSD
2. Configure auto-mount (fstab or systemd)
3. Move high-I/O paths to external SSD:
   - Create `/mnt/ssd` or similar mount point
   - Docker data directory → `/mnt/ssd/docker`
   - Logs → `/mnt/ssd/logs`
   - NAS data → `/mnt/ssd/nas`
4. Verify mount persistence across reboots

### Phase 3: Bare Metal Tools
1. Install Docker + Docker Compose
2. Configure Docker to use external SSD storage
3. Install Tailscale
4. Configure Tailscale subnet router
5. Install Homebrew (ARM-compatible)
6. Install uv via Homebrew
7. Install tmux and configure

### Phase 4: Pi-hole Deployment
1. Create Docker Compose file for Pi-hole
2. Configure volumes for persistence
3. Set static IP or DNS configuration
4. Deploy and verify
5. Configure Tailscale to use Pi-hole as DNS
6. Test ad blocking from Tailscale devices
7. Set up health checks and auto-restart

### Phase 5: Development Environment
1. Create development container Dockerfile
2. Install Claude Code in container
3. Install happy.engineering in container
4. Configure tmux in container
5. Create Docker Compose orchestration
6. Set up on-demand startup script
7. Test SSH workflow: connect to host → start dev container → attach tmux

### Phase 6: Monitoring Stack
1. Create Docker Compose stack:
   - Grafana
   - Loki
   - Promtail (configured for Docker logs)
   - Netdata
2. Configure Loki log collection from Docker
3. Set up Grafana dashboards:
   - System overview (Netdata data)
   - Docker container logs (Loki)
   - Container health and restarts
4. Configure log retention policies
5. Expose web UIs on Tailscale network
6. Create alerting rules (optional)

### Phase 7: File Sharing / NAS
1. Create Samba container or use existing solution
2. Mount external SSD storage into container
3. Configure shares and permissions
4. Test access from Tailscale devices
5. Document connection strings

### Phase 8: Self-Healing & Automation
1. Configure Docker restart policies (unless-stopped)
2. Enable Docker systemd service
3. Set up weekly system restart cron job
4. Create health check scripts
5. Configure watchdog timers (optional)
6. Test full power cycle recovery

### Phase 9: Backup Strategy
1. Identify critical data to backup:
   - Docker Compose files
   - Pi-hole configuration
   - User data/projects
2. Set up automated backups (rsync, Restic, or Borg)
3. Test restore procedure
4. Document backup locations

---

## Network Architecture

### Tailscale Integration
```
Internet
  ↓
Home Router (Ethernet) → Le Potato
  ↓
Tailscale Network (subnet router)
  ↓
All Tailscale Devices
```

**DNS Flow:**
1. Tailscale device queries DNS
2. Request routed to Le Potato (Tailscale DNS setting)
3. Pi-hole container processes query
4. Blocked if on blocklist, otherwise forwarded to upstream DNS
5. Response returned to device

### Port Mapping & Access
- Pi-hole Admin: `http://lepotato.local:8080` or Tailscale IP
- Grafana: `http://lepotato.local:3000` or Tailscale IP
- Netdata: `http://lepotato.local:19999` or Tailscale IP
- Samba/NAS: `smb://lepotato.local` or Tailscale IP
- SSH: Standard port 22 via Tailscale

---

## Configuration Management

### Docker Compose Structure
```
/opt/
  docker-compose/
    pi-hole/
      docker-compose.yml
      .env
    monitoring/
      docker-compose.yml
      grafana/
      loki/
      promtail/
    dev-env/
      docker-compose.yml
      Dockerfile
    nas/
      docker-compose.yml
```

### Systemd Integration
- Docker Compose services as systemd units
- Auto-start on boot
- Dependency ordering (storage before Docker)
- Graceful shutdown on restart

### Scheduled Maintenance
```bash
# Weekly restart cron job
0 3 * * 0 /sbin/shutdown -r +5 "Weekly maintenance restart in 5 minutes"
```

---

## Open Questions & Research Needed

### Hardware
- [ ] Le Potato exact model and USB boot capability
- [ ] USB 3.0 port availability and performance
- [ ] Power supply capacity for external SSDs
- [ ] Thermal management for 24/7 operation

### Software
- [ ] Pi-hole Docker networking: host mode vs macvlan vs bridge
- [ ] Grafana/Loki resource usage on ARM (RAM/CPU)
- [ ] Best Docker storage driver for ARM + ext4
- [ ] Claude Code container compatibility (architecture-specific builds?)
- [ ] happy.engineering ARM support

### Storage
- [ ] Optimal filesystem for external SSD (ext4 vs btrfs)
- [ ] `/var/log` relocation strategy
- [ ] Docker volume driver options
- [ ] Backup solution selection (Restic vs Borg vs rsync)

### Monitoring
- [ ] Log retention requirements (disk space vs history)
- [ ] Grafana dashboard templates for this use case
- [ ] Alert notification options (email, mobile?)

---

## Success Criteria

### Functional Requirements
- ✅ Can SSH to Le Potato from any Tailscale device
- ✅ Pi-hole blocks ads on all Tailscale-connected devices
- ✅ Can start development container and attach to tmux session
- ✅ NAS shares accessible from network
- ✅ Web UIs accessible for monitoring and management
- ✅ System survives power cycle without manual intervention
- ✅ Weekly restarts occur automatically

### Reliability Requirements
- ✅ All services auto-start on boot
- ✅ Docker containers restart on failure
- ✅ Monitoring shows service health in real-time
- ✅ Logs retained for at least 7 days
- ✅ microSD card longevity optimized (minimal writes)

### Usability Requirements
- ✅ Simple workflow to start dev environment
- ✅ Clear dashboard showing system health
- ✅ Easy to add new services via Docker Compose
- ✅ Documentation for common tasks

---

## Risks & Mitigations

### Risk: microSD Card Failure
- **Likelihood:** Medium (1-3 year lifespan)
- **Impact:** High (complete system down)
- **Mitigation:** 
  - Move high-write workloads to SSD
  - Keep monthly SD card image backups
  - Research USB boot as long-term solution
  - Document rebuild procedure

### Risk: Network Connectivity Loss
- **Likelihood:** Low
- **Impact:** High (lose remote access)
- **Mitigation:**
  - Configure fallback SSH access on local network
  - Set static IP as backup
  - Physical access to device if needed

### Risk: Docker Container Misconfiguration
- **Likelihood:** Medium
- **Impact:** Medium (service down but recoverable)
- **Mitigation:**
  - Version control all Docker Compose files
  - Test changes before deploying
  - Health checks and restart policies
  - Monitoring alerts on container failures

### Risk: Storage Exhaustion
- **Likelihood:** Medium (logs can grow)
- **Impact:** Medium (service degradation)
- **Mitigation:**
  - Log rotation and retention policies
  - Disk space monitoring with alerts
  - Weekly cleanup scripts

---

## Future Enhancements

### Short-term (Next 3-6 months)
- Additional Pi-hole instances for redundancy
- Automated backup testing
- Custom Grafana dashboards
- Media server (Plex/Jellyfin)

### Medium-term (6-12 months)
- Migrate to USB boot (if supported)
- RAID configuration for external SSDs
- VPN server (WireGuard) as alternative to Tailscale
- Home automation integration

### Long-term (1+ years)
- Kubernetes for orchestration (overkill but fun)
- GitOps workflow for configuration
- Multiple Le Potato devices for high availability
- Custom monitoring/alerting system

---

## Documentation Requirements

### Setup Documentation
- Complete installation guide with commands
- Configuration file templates
- Troubleshooting common issues

### Operations Documentation
- How to add new services
- How to start/stop dev environment
- How to access logs and metrics
- Backup and restore procedures

### Network Documentation
- Tailscale configuration details
- Pi-hole DNS setup
- Port mappings and firewall rules

---

## Next Steps

1. **Research Phase:** Answer all questions in "Open Questions" section
2. **Planning Phase:** Refine architecture based on research findings
3. **Procurement:** Purchase external SSD(s) if not already available
4. **Implementation:** Follow phase-by-phase guide
5. **Testing:** Verify all success criteria
6. **Documentation:** Create operational runbooks

---

## Notes

- This is a living document - update as implementation progresses
- Track decisions and their rationale
- Document any deviations from the plan
- Keep a changelog of what was tried and what worked

---

**End of Specification**
