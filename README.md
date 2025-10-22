# Le Potato Home Server

**Libre Computer AML-S905X-CC (Le Potato) 2GB**
**Status:** Planning → Implementation
**Version:** 2.0 (Research-Validated)
**Overall Confidence:** 🟢 HIGH (90%)

A research-validated home server project transforming the Le Potato single-board computer into a reliable, containerized server with remote access, network-wide ad blocking, monitoring, and development capabilities.

---

## 📋 Project Overview

### Primary Goals
- **24/7 Reliable Server** with self-healing Docker containers
- **Remote Access** via Tailscale VPN (no local interaction needed)
- **Network-Wide Ad Blocking** with Pi-hole accessible from anywhere
- **On-Demand Development** with Claude Code + happy.engineering
- **NAS Capabilities** using external SSD storage
- **Comprehensive Monitoring** with VictoriaLogs + Grafana + Netdata
- **Data Protection** with automated daily backups

### Hardware Specifications
- **Board:** Libre Computer AML-S905X-CC (Le Potato)
- **CPU:** Quad-core ARM Cortex-A53 @ 1.5GHz (64-bit ARM64)
- **RAM:** 2GB DDR3 (hard constraint)
- **Boot Storage:** 32GB eMMC module (4× faster than microSD)
- **Data Storage:** External SSDs via powered USB hub
- **Cooling:** Active cooling (heatsink + 3010 5V fan)
- **Network:** Gigabit Ethernet + Tailscale VPN

### Key Features
- ✅ **Pi-hole:** Network-wide ad/tracker blocking via Tailscale DNS
- ✅ **Tailscale:** Zero-config VPN for secure remote access
- ✅ **VictoriaLogs:** Lightweight log aggregation (87% less RAM than Loki)
- ✅ **Grafana:** Real-time monitoring dashboards with temperature/memory alerts
- ✅ **Netdata:** System metrics and Docker container monitoring
- ✅ **Development Container:** Claude Code + Node.js 20 environment
- ✅ **Automated Backups:** Daily rsync snapshots with 7-day retention
- ✅ **Self-Healing:** Docker restart policies for automatic recovery

---

## 🚀 Quick Start

### Current Status
The project is in the **Planning → Implementation** transition phase:

1. ✅ **Research Complete** - 17 prompts validated, all findings documented
2. ✅ **Architecture Finalized** - v2.0 specification ready
3. ⏳ **Hardware Acquisition** - Essential components identified
4. 📋 **Implementation Ready** - Phase-by-phase guide prepared

### Next Steps
1. Review the [Project Specification v2.0](docs/specs/le-potato-server-spec-v2.md)
2. Acquire [essential hardware](#hardware-requirements) ($180-232)
3. Follow [implementation phases](#implementation-phases) (4-6 weeks)

---

## 📁 Project Structure

```
HomeServer/
├── README.md                          # This file
├── CLAUDE.md                          # Claude AI workflow guidelines
├── GIT_COMMIT_STYLE.md               # Git commit conventions
├── QUICK-REFERENCE.md                # Quick reference guide
├── docs/                             # Documentation
│   ├── specs/                        # Project specifications
│   │   └── le-potato-server-spec-v2.md  # Complete v2.0 specification
│   ├── planning/                     # Research planning documents
│   └── workflows/                    # Workflow documentation
├── archive/                          # Research findings archive
│   └── research-findings/            # 17 completed research documents
└── ClaudeUsage/                      # Claude Code usage tracking
```

### Key Documents
- **[le-potato-server-spec-v2.md](docs/specs/le-potato-server-spec-v2.md)** - Complete project specification with:
  - Hardware requirements and validated components
  - Service architecture and container configurations
  - Resource budget for 2GB RAM constraint
  - Day-by-day implementation guide
  - Operational procedures and troubleshooting

- **[archive/research-findings/](archive/)** - Research findings from:
  - Hardware validation (USB, power, thermal, boot storage)
  - Software compatibility (ARM64, Docker, monitoring stack)
  - Storage architecture (filesystems, Docker volumes, backups)
  - Monitoring solutions (log retention, dashboards, alerts)

---

## 🔧 Hardware Requirements

### Essential Components ($180-232)
All components validated through research for 24/7 reliability:

| Component | Specification | Purpose | Est. Cost |
|-----------|--------------|---------|-----------|
| **eMMC Module** | 32GB (min 16GB) | Boot storage (4× faster than microSD) | $20-25 |
| **Active Cooling** | Heatsink + 3010 5V fan | Prevent throttling (mandatory) | $8-15 |
| **Ventilated Case** | LoveRPi Active Cooling Case | Housing + airflow | $10-15 |
| **Powered USB Hub** | Sabrent/Anker 36W+ (7-port) | Stable SSD power (mandatory) | $25-35 |
| **Power Supply** | GenBasic 5V 3A MicroUSB | Board power | $10-12 |
| **Primary SSD** | 2.5" 1TB SATA SSD | Docker volumes + data | $60-80 |
| **USB-SATA Adapter** | USB 3.0 (backwards compatible) | SSD connection | $12-15 |

### Recommended Components ($112-150)
Strong value-add for production use:

| Component | Purpose | Est. Cost |
|-----------|---------|-----------|
| **Secondary SSD (1TB)** | Backups + logs | $60-80 |
| **Second USB-SATA** | Secondary SSD connection | $12-15 |
| **Thermal Camera** | Cooling verification | $25-35 |
| **USB Power Meter** | Power troubleshooting | $15-20 |

**Total Budget:**
- Minimum viable: **$180-232** (essential only)
- Recommended: **$300-392** (essential + recommended)

### Why These Specific Components?

**eMMC Boot (Research Finding HW-01):**
- 140+ MB/s read/write vs 35 MB/s microSD
- Better reliability and longevity
- Faster boot times (<60 seconds)

**Active Cooling (Research Finding HW-04):**
- Without cooling: CPU throttles to 100MHz (97% performance loss!)
- With cooling: Maintains 1.5GHz under load
- Keeps temps <55°C for 24/7 operation

**Powered USB Hub (Research Finding HW-03):**
- Direct USB power WILL cause boot loops and crashes
- SSD power spikes during spin-up exceed board capacity
- Mandatory for storage stability

---

## 🏗️ Implementation Phases

### Phase 0: Hardware Acquisition & Validation (1-2 days)
- Acquire all essential components
- Assemble hardware with active cooling
- Validate power, thermal, and USB stability
- **Success:** Idle temp <45°C, no USB errors for 30 minutes

### Phase 1: Base System Setup (Week 1)
- Flash Ubuntu Server 22.04 ARM64 to eMMC
- Configure external SSD storage with ext4
- Install Docker with external data-root
- Set up thermal monitoring
- **Success:** System boots reliably, temps <55°C under load

### Phase 2: Core Services Deployment (Week 2)
- Deploy Pi-hole + Tailscale containers
- Configure Tailscale DNS for network-wide ad blocking
- 48-hour stability testing
- **Success:** Pi-hole accessible via Tailscale, ads blocked, no restarts

### Phase 3: Monitoring Stack Deployment (Week 3)
- Deploy VictoriaLogs + Grafana + Netdata
- Configure temperature/memory alert rules
- Import monitoring dashboards
- **Success:** All metrics visible, alerts functional, <1.6GB RAM total

### Phase 4: Development Environment (Week 4)
- Build development container with Claude Code v0.2.114
- Implement mode switching (dev ↔ monitoring)
- Test Claude Code ARM64 compatibility
- **Success:** Dev environment functional, memory <1.8GB in dev mode

### Phase 5: NAS / File Sharing (Optional, Week 5)
- Deploy Samba container for file sharing
- Test access via Tailscale
- **Success:** Network file access working

### Phase 6: Backup Automation (Week 6)
- Set up daily rsync backup script
- Configure 7-day retention with hard-link snapshots
- Test restoration procedure
- **Success:** Automated backups running, restoration verified

**Total Timeline:** 4-6 weeks from hardware to full deployment

---

## 💾 Service Architecture

### Always-Running Services
```
System Base (Ubuntu 22.04):        ~400MB RAM
Pi-hole + Tailscale:               ~200MB RAM
VictoriaLogs:                      ~200MB RAM
Grafana:                           ~150MB RAM
vmagent (log collection):           ~50MB RAM
Netdata (system metrics):          ~120MB RAM
System buffers:                    ~200MB RAM
----------------------------------------
Total:                             ~1320MB RAM
Available:                          ~680MB RAM
```

### On-Demand Services (Mutually Exclusive)
Due to 2GB RAM constraint, development and monitoring cannot run simultaneously:

**Development Mode:**
- Development container (Claude Code + Node.js): ~400MB RAM
- Total system usage: ~1200MB
- Available: ~800MB

**Monitoring Mode:**
- Full monitoring stack: ~670MB RAM
- Total system usage: ~1470MB
- Available: ~530MB

### Mode Switching
```bash
# Switch to development
~/switch-to-dev.sh
docker exec -it devenv bash

# Switch back to monitoring
~/switch-to-monitoring.sh
```

---

## 🎯 Key Research Findings

The v2.0 specification incorporates findings from 17 research prompts:

### Critical Architectural Changes
- **Monitoring Stack:** VictoriaLogs instead of Loki (87% RAM reduction)
- **Boot Strategy:** eMMC instead of microSD (4× faster, more reliable)
- **Cooling:** Active cooling mandatory (prevents 97% performance loss)
- **Power:** Powered USB hub required (prevents boot loops and crashes)
- **Filesystem:** ext4 confirmed over btrfs (better Docker performance)

### Validated Compatibility
- ✅ Pi-hole + Tailscale: Service container networking validated
- ✅ Claude Code v0.2.114: ARM64 compatible
- ✅ VictoriaLogs: ARM64 support confirmed, LogQL compatible
- ✅ Docker on ARM64: Full compatibility with overlay2 storage driver

### Resource Management
- Memory limits configured for all containers
- Development/monitoring mutually exclusive operation
- 8GB swap on external SSD as safety net
- Log rotation and retention policies

---

## 📊 Project Status

### Completed
- ✅ Research phase (17 prompts validated)
- ✅ Architecture design v2.0
- ✅ Hardware requirements validated
- ✅ Implementation plan prepared
- ✅ Docker configurations drafted
- ✅ Monitoring dashboards identified

### In Progress
- ⏳ Hardware acquisition
- ⏳ Phase 0 validation testing

### Planned
- 📋 Base system installation
- 📋 Service deployment
- 📋 Production validation
- 📋 Long-term stability testing

---

## 🔬 Technical Details

### Operating System
- **Ubuntu Server 22.04 LTS ARM64**
- 5-year LTS support
- Excellent ARM compatibility
- Mature Docker ecosystem

### Container Strategy
- **Docker + Docker Compose**
- Data-root on external SSD
- overlay2 storage driver
- systemd integration for auto-start
- Memory limits for all containers

### Storage Layout
```
/dev/mmcblk1p1 (eMMC):        /                (OS, 32GB)
/dev/sda1 (USB SSD):          /mnt/storage-primary    (Docker, 1TB)
/dev/sdb1 (USB SSD):          /mnt/storage-secondary  (Logs/backups, 1TB)
```

### Network Architecture
- Tailscale VPN for remote access
- Pi-hole DNS accessible to all Tailscale devices
- No inbound firewall rules needed
- Local network access as fallback

---

## 📚 Documentation

### Specifications
- [Project Specification v2.0](docs/specs/le-potato-server-spec-v2.md) - Complete implementation guide

### Research Findings
Located in `archive/research-findings/`:
- Hardware validation (4 prompts)
- Software compatibility (5 prompts)
- Storage architecture (4 prompts)
- Monitoring solutions (3 prompts)
- Synthesis and integration (1 prompt)

### Operational Guides
All included in [le-potato-server-spec-v2.md](docs/specs/le-potato-server-spec-v2.md):
- Daily health checks
- Mode switching procedures
- Emergency troubleshooting
- Backup and restoration
- Service management

---

## ⚠️ Design Constraints

### Hardware Limitations
- **2GB RAM** - Hard limit, requires careful service selection
- **USB 2.0 only** - 35-40 MB/s max throughput
- **CPU throttling** - Drops to 100MHz at 60°C without cooling
- **USB power budget** - 2A (10W) shared across all ports

### Design Solutions
- VictoriaLogs for monitoring (200MB vs 1.5GB)
- eMMC boot for reliability and speed
- Powered USB hub for storage stability
- Active cooling for thermal management
- Mutually exclusive dev/monitoring modes

---

## 🎯 Success Metrics

### Phase 1-2 (Weeks 1-2)
- [ ] System boots from eMMC in <60 seconds
- [ ] Pi-hole accessible via Tailscale
- [ ] Temperatures <55°C under load
- [ ] No container restarts for 48 hours

### Phase 3-4 (Weeks 3-4)
- [ ] Monitoring stack operational
- [ ] Temperature alerts functional
- [ ] Development environment working
- [ ] Total RAM <1.8GB in any mode

### Long-Term (Months 1-3)
- [ ] Uptime >99% (excluding maintenance)
- [ ] Zero data loss events
- [ ] No thermal throttling
- [ ] Backups running daily
- [ ] All services stable

---

## 🚦 Project Confidence

**Overall: 🟢 HIGH (90%)**

All critical path research completed with high confidence:
- ✅ Hardware compatibility validated
- ✅ Software stack confirmed functional
- ✅ Resource budget verified
- ✅ Thermal requirements understood
- ✅ Power architecture designed
- ✅ No identified showstoppers

**Ready for implementation.**

---

## 🤝 Contributing

This is a personal home server project, but the research findings and implementation guide may be useful for similar Le Potato or ARM64 SBC projects.

### Useful Resources
- **Libre Computer Forum:** https://forum.librecomputer.io/
- **Project Specification:** [docs/specs/le-potato-server-spec-v2.md](docs/specs/le-potato-server-spec-v2.md)
- **Research Findings:** [archive/](archive/)

---

## 📝 Version History

### v2.0 (October 2025) - Research-Validated
- Completed 17 research prompts
- Major architectural changes based on findings
- Added eMMC boot requirement
- Added active cooling requirement
- Added powered USB hub requirement
- Switched to VictoriaLogs monitoring
- Confirmed ext4 filesystem choice
- Validated Claude Code ARM64 compatibility
- Added detailed implementation phases

### v1.0 (October 2025) - Initial Planning
- Initial project specification
- Research prompts identified
- Architecture proposed (unvalidated)

---

## 📄 License

Personal project - no formal license. Use research findings and documentation as reference for your own projects.

---

**Status:** ✅ Ready for Implementation
**Next Step:** Hardware Acquisition (Phase 0)
**Estimated Time to Production:** 4-6 weeks

---

*Built with research, validated with data, deployed with confidence.*
