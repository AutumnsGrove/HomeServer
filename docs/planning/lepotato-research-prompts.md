# Le Potato Home Server - Research Agent Metaprompts

**Board Model:** Libre Computer AML-S905X-CC (Le Potato) 2GB RAM  
**CPU:** Quad-core ARM Cortex-A53 @ 1.5GHz (64-bit)  
**GPU:** ARM Mali-450 @ 750MHz  
**Target OS:** Ubuntu Server 22.04 LTS ARM64  
**Date:** October 11, 2025

---

## HARDWARE RESEARCH PROMPTS

### HW-01: USB Boot Capability Investigation

**Prompt:**
```
Research the USB boot capabilities of the Libre Computer AML-S905X-CC (Le Potato) board with the following constraints:

SPECIFIC HARDWARE:
- Model: AML-S905X-CC
- SoC: Amlogic S905X
- Current boot: microSD card
- UEFI/GRUB support mentioned in specs

RESEARCH OBJECTIVES:
1. Can this specific board boot from USB storage (USB 2.0 or 3.0)?
2. What is the official boot order priority (microSD, eMMC, USB, SPI)?
3. Does it require firmware updates or bootloader modifications?
4. Are there BIOS/UEFI settings to enable USB boot?
5. What are the documented success rates from community reports?

SEARCH STRATEGY:
- Look for Libre Computer official documentation
- Search Armbian/Ubuntu forums for "AML-S905X-CC USB boot"
- Check Reddit r/selfhosted, r/homelab for real-world experiences
- Examine GitHub issues on libre-computer repositories
- Look for YouTube tutorials or blog posts with step-by-step guides

DESIRED OUTPUT:
- Clear YES/NO/EXPERIMENTAL answer on USB boot support
- Step-by-step procedure if supported
- Known limitations or caveats
- Estimated reliability for 24/7 server use
- Alternative solutions if USB boot is not viable
```

---

### HW-02: USB Port Specifications and Performance

**Prompt:**
```
Investigate the USB port specifications and real-world performance of the Libre Computer AML-S905X-CC (Le Potato) for external SSD usage in a 24/7 server context.

HARDWARE CONTEXT:
- Board: AML-S905X-CC 2GB model
- Planned use: 1-3 external SSDs for Docker volumes, logs, and NAS storage
- Expected workload: Continuous read/write for logging, moderate I/O for Pi-hole and development

RESEARCH QUESTIONS:
1. USB port inventory: How many USB 2.0 vs USB 3.0 ports?
2. USB 3.0 controller specifications and chipset
3. Real-world SSD performance benchmarks (read/write speeds)
4. Can multiple SSDs be connected simultaneously via hub?
5. USB power delivery limits per port and total board capacity
6. Known USB stability issues under continuous 24/7 load
7. Recommended USB hub models if needed (powered vs unpowered)

SEARCH STRATEGY:
- Official Libre Computer specs and schematics
- Search for "AML-S905X-CC USB 3.0 SSD benchmark"
- Look for forum posts about external storage performance
- Check for USB power consumption data
- Search for compatible USB hub recommendations

CRITICAL FACTORS:
- Stability over performance (24/7 reliability required)
- Power draw considerations
- Real-world longevity reports

DESIRED OUTPUT:
- Port inventory with speeds
- Expected SSD throughput (MB/s)
- Power budget analysis
- USB hub recommendations if needed
- Any showstopper issues to be aware of
```

---

### HW-03: Power Supply Requirements for External SSDs

**Prompt:**
```
Analyze power supply requirements for running the Le Potato board with multiple external SSDs in a 24/7 home server configuration.

SYSTEM CONFIGURATION:
- Board: Libre Computer AML-S905X-CC (2GB)
- Board power spec: Uses "half the power of Pi 3 B+" per manufacturer
- External storage: Planning 1-3 external SSDs (2.5" SATA via USB)
- Additional load: Small cooling fan already installed
- Runtime: 24/7 continuous operation

RESEARCH QUESTIONS:
1. What is the typical power draw of the Le Potato under load?
2. What is the official recommended power supply specification (voltage/amperage)?
3. What is the typical power draw of 2.5" SSDs via USB?
4. Should SSDs be self-powered or can they be bus-powered from Le Potato USB?
5. What is the maximum safe total power draw from USB ports?
6. What is the recommended power supply wattage with overhead margin?
7. Are there known issues with undervoltage affecting USB storage?

SEARCH STRATEGY:
- Look for official Libre Computer power specifications
- Search for "AML-S905X-CC power consumption"
- Compare with Raspberry Pi 3 B+ power specs (baseline reference)
- Research typical 2.5" SSD power requirements
- Look for USB power delivery specifications on this board
- Search forums for power-related stability issues

CRITICAL CONSIDERATIONS:
- Need reliable 24/7 operation (no brownouts/crashes)
- Multiple USB devices (SSDs + potential hub)
- Headroom for peak load scenarios

DESIRED OUTPUT:
- Recommended power supply specifications (V/A)
- Whether external SSD power is needed
- Total system power budget estimate
- Specific PSU product recommendations if available
- Red flags or known power issues
```

---

### HW-04: Thermal Management for 24/7 Operation

**Prompt:**
```
Research thermal management requirements for running the Le Potato as a 24/7 home server with moderate continuous workload.

SYSTEM CONTEXT:
- Board: AML-S905X-CC 2GB (Amlogic S905X SoC)
- Current cooling: Small fan + external case (unspecified)
- Workload profile:
  * Continuous: Pi-hole (DNS filtering), Tailscale VPN routing
  * Moderate: Docker containers (Grafana, Loki, Samba)
  * Occasional: Development environment, file transfers
  * CPU: Quad-core ARM Cortex-A53 @ 1.5GHz
- Runtime: 24/7/365 in typical indoor environment (20-25°C ambient)

RESEARCH QUESTIONS:
1. What are the normal operating temperatures for S905X under continuous load?
2. What is the thermal throttling threshold?
3. Is passive cooling sufficient, or is active cooling required?
4. What are the recommended heatsink specifications?
5. What are the recommended fan specifications (size, CFM, noise)?
6. Are there documented cases of thermal issues in 24/7 server scenarios?
7. What are the case recommendations for optimal airflow?
8. Do Docker workloads significantly increase thermal load?

SEARCH STRATEGY:
- Search for "Amlogic S905X operating temperature"
- Look for "Le Potato thermal management" or cooling guides
- Search forums for 24/7 server thermal experiences
- Look for temperature monitoring data from real deployments
- Search for recommended cases with cooling solutions

LONGEVITY CONSIDERATIONS:
- Will current cooling setup handle 1-3 years of continuous use?
- What are early warning signs of inadequate cooling?
- Should thermal monitoring be part of the monitoring stack?

DESIRED OUTPUT:
- Safe operating temperature ranges
- Current cooling adequacy assessment
- Specific cooling upgrade recommendations if needed
- Temperature monitoring tool recommendations
- Case/airflow optimization tips
```

---

## SOFTWARE RESEARCH PROMPTS

### SW-01: Pi-hole Docker Networking Architecture

**Prompt:**
```
Research the optimal Docker networking configuration for running Pi-hole on the Le Potato as the primary Tailscale DNS resolver.

ARCHITECTURE CONTEXT:
- Platform: Ubuntu Server 22.04 ARM64 on Le Potato (2GB RAM)
- Pi-hole role: Network-wide ad blocker for Tailscale network
- Integration: Pi-hole as Tailscale DNS upstream
- Access pattern: DNS queries from Tailscale devices → Tailscale → Pi-hole
- Admin access: Web UI via Tailscale IP address

NETWORKING OPTIONS TO EVALUATE:
1. **Host Network Mode** (--network host)
   - Pros/cons for DNS performance
   - Port conflict considerations
   - Security implications
   
2. **Macvlan Network**
   - Separate IP address on LAN
   - Complexity vs benefits
   - Compatibility with Tailscale subnet routing
   
3. **Bridge Network with Port Mapping**
   - Standard Docker approach
   - DNS port 53 forwarding
   - Performance considerations

SPECIFIC RESEARCH QUESTIONS:
1. Which networking mode works best with Tailscale subnet router?
2. Are there performance differences for DNS queries (latency)?
3. Which mode is most reliable for 24/7 operation?
4. How does each mode handle DHCP if needed?
5. What are the configuration complexities of each approach?
6. Are there known issues with Pi-hole + Tailscale + specific network modes?

SEARCH STRATEGY:
- Search for "Pi-hole Docker Tailscale networking"
- Look for official Pi-hole Docker documentation on networking
- Search for "macvlan vs host network Pi-hole"
- Look for Reddit/forum discussions on this specific combination
- Find Docker Compose examples for Pi-hole + VPN scenarios

DESIRED OUTPUT:
- Recommended networking mode with rationale
- Sample Docker Compose configuration
- Step-by-step Tailscale DNS integration guide
- Known issues and workarounds
- Performance comparison data if available
```

---

### SW-02: Grafana/Loki Resource Requirements on ARM

**Prompt:**
```
Assess the feasibility and resource requirements of running Grafana + Loki monitoring stack on the Le Potato's limited hardware.

HARDWARE CONSTRAINTS:
- Board: Le Potato (AML-S905X-CC)
- RAM: 2GB DDR3 total (shared with OS and other services)
- CPU: Quad-core ARM Cortex-A53 @ 1.5GHz
- Architecture: ARM64
- Storage: External SSD (adequate I/O)

CONCURRENT SERVICES:
- Pi-hole (primary service, critical)
- Tailscale (bare metal)
- Development container (on-demand)
- Samba/NAS (planned)
- System overhead (Ubuntu Server)

MONITORING STACK COMPONENTS:
- Grafana (visualization)
- Loki (log aggregation)
- Promtail (log shipper)
- Netdata (system metrics) - optional if resources tight

RESEARCH QUESTIONS:
1. What is the minimum RAM requirement for Grafana on ARM?
2. What is the minimum RAM requirement for Loki on ARM?
3. What are realistic resource allocations for each component?
4. Are there lightweight alternatives to Grafana/Loki for ARM?
5. Can Grafana/Loki run efficiently with Docker memory limits?
6. What is the CPU overhead during normal operation vs query time?
7. Are there ARM-specific performance issues or limitations?
8. What is the storage overhead for log retention (7-14 days)?

SEARCH STRATEGY:
- Search for "Grafana ARM minimum requirements"
- Look for "Loki Raspberry Pi 2GB RAM"
- Search for "lightweight monitoring alternatives ARM"
- Look for Docker Compose examples with resource limits
- Search forums for real-world deployments on similar hardware

CRITICAL SUCCESS FACTORS:
- Must not starve Pi-hole of resources (primary service)
- Must remain stable under continuous 24/7 operation
- Acceptable to use scaled-down configurations

DESIRED OUTPUT:
- Feasibility assessment (GO/NO-GO/GO-WITH-LIMITS)
- Recommended resource allocations per container
- Lightweight alternatives if full stack won't fit
- Docker Compose configuration with memory/CPU limits
- Performance expectations and limitations
```

---

### SW-03: Docker Storage Driver for ARM + ext4

**Prompt:**
```
Determine the optimal Docker storage driver configuration for the Le Potato running Ubuntu Server with external SSD storage.

SYSTEM CONFIGURATION:
- OS: Ubuntu Server 22.04 LTS ARM64
- Docker storage location: External SSD (ext4 filesystem)
- Hardware: Le Potato (ARM Cortex-A53)
- Use case: Multiple containers with persistent volumes, 24/7 operation
- Workload: Moderate I/O (logs, DNS queries, file serving)

CONTEXT:
- Default Ubuntu Docker driver: overlay2
- External SSD mounted at /mnt/ssd/docker
- Need reliable performance and data integrity
- Not using btrfs or zfs on the SSD

RESEARCH QUESTIONS:
1. Is overlay2 the recommended driver for ARM + ext4?
2. Are there performance differences between storage drivers on ARM?
3. Are there ARM-specific considerations for storage drivers?
4. What are the data integrity implications of each driver?
5. Does the storage driver choice affect SSD longevity?
6. Are there known issues with overlay2 on external USB storage?
7. What are the best practices for Docker data-root on external storage?

SEARCH STRATEGY:
- Search for "Docker storage driver ARM recommendations"
- Look for "overlay2 vs devicemapper ARM performance"
- Search for "Docker external USB storage best practices"
- Look for Ubuntu ARM Docker documentation
- Search for known issues with overlay2 on USB storage

CONFIGURATION CONSIDERATIONS:
- Need to modify /etc/docker/daemon.json
- Must ensure driver is compatible with ARM architecture
- Should prioritize stability over maximum performance

DESIRED OUTPUT:
- Recommended storage driver (likely overlay2, but confirm)
- Sample /etc/docker/daemon.json configuration
- Any ARM or USB-specific optimizations
- Known issues and workarounds
- Alternative drivers to avoid
```

---

### SW-04: Claude Code ARM Compatibility

**Prompt:**
```
Research the compatibility and installation requirements for running Claude Code CLI on ARM64 architecture within a Docker container on the Le Potato.

PROJECT CONTEXT:
- Target: Development environment Docker container on Le Potato
- Host: Ubuntu Server 22.04 ARM64
- Container base: Ubuntu or Debian ARM64 image
- Purpose: Remote development with Claude Code + happy.engineering
- Access pattern: SSH to host → docker exec into container → tmux session

CLAUDE CODE INFORMATION:
- Product: Anthropic's command-line tool for agentic coding
- Installation: Typically via npm/node or standalone binary
- Documentation: https://docs.claude.com/en/docs/claude-code

RESEARCH QUESTIONS:
1. Is Claude Code officially supported on ARM64/aarch64 architecture?
2. What is the installation method for ARM (npm, binary, source)?
3. Does Claude Code depend on x86-specific libraries?
4. Are there known issues running Claude Code on ARM SBCs?
5. What are the minimum resource requirements (RAM/CPU)?
6. Can it run efficiently in a containerized environment?
7. Are there ARM-specific installation steps or workarounds?
8. What Node.js version is required, and is it ARM-compatible?

SEARCH STRATEGY:
- Check official Anthropic documentation at docs.claude.com
- Search for "Claude Code ARM installation"
- Look for GitHub issues mentioning ARM or aarch64
- Search forums for Raspberry Pi or ARM deployment experiences
- Check npm package details for architecture support

FALLBACK SCENARIOS:
- If native ARM not supported, investigate emulation viability (qemu-user-static)
- Alternative: Use web-based Claude interface as fallback

DESIRED OUTPUT:
- Compatibility status (NATIVE/EMULATED/NOT-SUPPORTED)
- Installation procedure for ARM64
- Dockerfile snippet for Claude Code installation
- Resource requirements and performance expectations
- Known limitations or workarounds
```

---

### SW-05: happy.engineering ARM Support

**Prompt:**
```
Investigate the ARM64 compatibility and installation requirements for happy.engineering within the Le Potato development container.

PROJECT CONTEXT:
- Target environment: Docker container on Ubuntu Server 22.04 ARM64
- Hardware: Le Potato (ARM Cortex-A53, 2GB RAM)
- Use case: Companion tool to Claude Code in development environment
- Container access: SSH → docker exec → tmux

HAPPY.ENGINEERING BACKGROUND:
- Tool for enhancing coding workflows (research exact purpose if unclear)
- Likely Python or Node.js based (confirm during research)
- May have dependencies on system libraries

RESEARCH QUESTIONS:
1. What is happy.engineering exactly? (if not well-known, clarify purpose)
2. Is it officially compatible with ARM64 architecture?
3. What are the installation requirements (pip, npm, cargo, etc.)?
4. Does it have native ARM binaries or requires compilation?
5. Are there ARM-specific dependencies or libraries needed?
6. What are the minimum system requirements?
7. Can it run in a containerized environment?
8. Are there known issues or limitations on ARM?

SEARCH STRATEGY:
- Search for "happy.engineering ARM installation"
- Look for official documentation or GitHub repository
- Check package manager listings (pip, npm, cargo)
- Search for Raspberry Pi or ARM deployment examples
- Look for container/Docker images that include it

COMPATIBILITY ASSESSMENT:
- Native ARM support: Best case
- Requires compilation: Acceptable if straightforward
- X86-only: Investigate alternatives or skip

DESIRED OUTPUT:
- Compatibility status and installation method
- Dockerfile snippet for installation
- Dependencies and system requirements
- Known issues on ARM platforms
- Alternative tools if incompatible
```

---

## STORAGE RESEARCH PROMPTS

### ST-01: Filesystem Choice for External SSD

**Prompt:**
```
Compare ext4 vs btrfs for the external SSD use case on Le Potato home server, focusing on reliability, performance, and features for 24/7 Docker workloads.

STORAGE CONTEXT:
- Device: External 2TB SSD (2.5" via USB 3.0)
- Use cases:
  * Docker volumes (high I/O, many small files)
  * System logs (sequential writes, rotation)
  * NAS file storage (large files, moderate access)
- Platform: Ubuntu Server 22.04 ARM64 on Le Potato (2GB RAM)
- Priority: Reliability > Features > Performance

FILESYSTEM OPTIONS:
1. **ext4**
   - Pros: Mature, stable, universal support, low overhead
   - Cons: No snapshots, no data integrity checks
   
2. **btrfs**
   - Pros: Snapshots, checksums, compression, subvolumes
   - Cons: Higher complexity, RAM overhead, maturity concerns

RESEARCH QUESTIONS:
1. What is the real-world reliability of btrfs on ARM in 2025?
2. What is the RAM overhead of btrfs vs ext4 for this use case?
3. Are btrfs snapshots practical on 2GB RAM system?
4. How does Docker perform on btrfs vs ext4?
5. Are there USB storage-specific considerations for btrfs?
6. What are the failure modes of each filesystem on sudden power loss?
7. Is btrfs scrubbing viable on USB-attached storage?
8. What is the administrative overhead of maintaining btrfs?

SPECIFIC CONCERNS:
- 2GB RAM limitation (btrfs needs more memory)
- USB storage vs internal SATA (btrfs may be optimized for SATA)
- Power loss scenarios (Le Potato may lose power unexpectedly)
- Ease of recovery if filesystem corruption occurs

SEARCH STRATEGY:
- Search for "btrfs vs ext4 Raspberry Pi Docker"
- Look for "btrfs RAM requirements low memory"
- Search for "btrfs USB external storage reliability"
- Look for Docker-specific filesystem benchmarks on ARM
- Search for power loss resilience comparisons

DESIRED OUTPUT:
- Clear recommendation (ext4 or btrfs) with rationale
- If ext4: Backup strategy recommendations
- If btrfs: Configuration guidance and limitations
- Performance implications
- Recovery procedures for each filesystem
```

---

### ST-02: /var/log Relocation Strategy

**Prompt:**
```
Research best practices and methods for relocating /var/log to external SSD to minimize microSD card wear on the Le Potato.

SYSTEM CONTEXT:
- OS: Ubuntu Server 22.04 ARM64
- Boot device: 256GB SanDisk microSD card
- External storage: SSD mounted at /mnt/ssd
- Goal: Extend microSD longevity by moving high-write workloads
- Services: Docker containers, systemd services, kernel logs

RELOCATION OPTIONS TO EVALUATE:
1. **Symbolic link** (/var/log → /mnt/ssd/log)
2. **Bind mount** (mount --bind in fstab)
3. **systemd mount unit**
4. **rsyslog/journald redirection**

RESEARCH QUESTIONS:
1. What is the recommended method for /var/log relocation on Ubuntu?
2. Are there boot-order dependencies (mounting before logging starts)?
3. How to handle temporary unavailability of external SSD?
4. What are the implications for systemd-journald?
5. Should journald still write to SD initially, then rotate to SSD?
6. How to migrate existing logs safely?
7. Are there specific considerations for Docker container logs?
8. What happens if external SSD is not present at boot?

SEARCH STRATEGY:
- Search for "Ubuntu move /var/log external drive"
- Look for "systemd journald external storage"
- Search for "bind mount /var/log best practices"
- Look for ARM/Raspberry Pi guides on SD card wear reduction
- Search for boot order dependencies with external storage

CRITICAL REQUIREMENTS:
- System must boot safely if SSD is disconnected
- No log data loss during relocation
- Minimal performance impact
- Easy troubleshooting if issues arise

DESIRED OUTPUT:
- Recommended relocation method with rationale
- Step-by-step migration procedure
- fstab or systemd mount unit configuration
- Fallback behavior configuration
- Testing procedure to verify it works
```

---

### ST-03: Docker Volume Driver Options

**Prompt:**
```
Research Docker volume driver options and best practices for persistent storage on external SSD with the Le Potato home server setup.

STORAGE ARCHITECTURE:
- Docker data-root: /mnt/ssd/docker
- External SSD filesystem: ext4 (or btrfs per ST-01 results)
- Connection: USB 3.0
- Use cases: Pi-hole config, Grafana dashboards, Loki indexes, dev project files

VOLUME MANAGEMENT OPTIONS:
1. **Default local driver** (bind mounts or named volumes)
2. **Third-party drivers** (local-persist, convoy, etc.)
3. **Direct bind mounts** from host filesystem
4. **Named volumes** managed by Docker

RESEARCH QUESTIONS:
1. Is the default local driver sufficient for this use case?
2. Should volumes be created explicitly or let Docker manage them?
3. What are best practices for volume backup strategies?
4. How to ensure volumes survive Docker reinstalls/upgrades?
5. What are the performance implications of different volume types?
6. Are there ARM-specific volume driver considerations?
7. How to handle volume permissions for non-root containers?
8. What are best practices for volume naming/organization?

SPECIFIC USE CASES:
- Pi-hole: Config persistence (/etc/pihole, /etc/dnsmasq.d)
- Grafana: Dashboard and datasource config
- Loki: Indexes and chunks
- Development: Source code, git repos, build artifacts

SEARCH STRATEGY:
- Search for "Docker volumes best practices production"
- Look for "Docker volume drivers comparison"
- Search for "Docker external storage reliability"
- Look for backup strategies for Docker volumes
- Search for ARM/SBC-specific volume considerations

DESIRED OUTPUT:
- Recommended volume management approach
- Docker Compose volume configuration examples
- Backup and restore procedures
- Directory structure recommendations
- Troubleshooting tips for volume issues
```

---

### ST-04: Backup Solution Selection

**Prompt:**
```
Evaluate and recommend a backup solution for the Le Potato home server, comparing Restic, Borg, and rsync for this specific use case.

BACKUP REQUIREMENTS:
- Source data:
  * Docker Compose files (/opt/docker-compose/*)
  * Docker volumes (/mnt/ssd/docker/volumes/)
  * Pi-hole configuration
  * System configuration (/etc/)
  * User data and projects
- Destination: Either cloud storage (B2, S3) or second external drive
- Frequency: Daily incremental, weekly full
- Retention: 30 days minimum
- Automation: Cron-based, no manual intervention

PLATFORM CONSTRAINTS:
- Hardware: Le Potato, 2GB RAM, ARM64
- Network: Residential internet (upload may be limited)
- Resources: Low CPU/memory overhead preferred
- Complexity: Should be maintainable long-term

SOLUTIONS TO COMPARE:

1. **Restic**
   - Deduplication, encryption, multiple backends
   - Resource usage on ARM?
   - Ease of restoration
   
2. **Borg**
   - Deduplication, compression, encryption
   - Local-first design
   - ARM compatibility
   
3. **rsync**
   - Simple, reliable, well-understood
   - No built-in encryption or deduplication
   - Easy scripting and scheduling

RESEARCH QUESTIONS:
1. Which tool has best ARM performance with 2GB RAM?
2. Which is most reliable for automated, unattended backups?
3. What are the real-world resource requirements of each?
4. Which has best Docker volume backup support?
5. How do restore procedures compare in complexity?
6. Are there ARM-specific issues with any of these tools?
7. What are the cloud storage costs for typical backup size?
8. Which tool has best error handling and alerting?

SEARCH STRATEGY:
- Search for "Restic vs Borg vs rsync comparison"
- Look for "Restic Raspberry Pi performance"
- Search for "Docker backup best practices"
- Look for automated backup scripts for each tool
- Search for reliability reports and failure modes

DESIRED OUTPUT:
- Recommended backup solution with justification
- Estimated resource usage (CPU/RAM/bandwidth)
- Sample configuration/script
- Restore procedure documentation
- Monitoring/alerting recommendations
```

---

## MONITORING RESEARCH PROMPTS

### MON-01: Log Retention Requirements

**Prompt:**
```
Determine appropriate log retention policies for the Le Potato monitoring stack, balancing storage constraints with operational visibility.

SYSTEM CONTEXT:
- Storage: External SSD (2TB capacity, but multi-purpose)
- Monitoring stack: Grafana + Loki + Promtail
- Log sources: Docker containers (Pi-hole, Samba, dev env), systemd services
- Workload: Pi-hole logs (high volume), application logs (moderate), system logs (low)

RETENTION GOALS:
- Troubleshooting window: Able to investigate issues that occurred recently
- Compliance: None (personal home server)
- Storage efficiency: Don't want logs consuming 500GB+ of SSD space
- Performance: Loki query performance degrades with too much data

RESEARCH QUESTIONS:
1. What is a reasonable retention period for home server logs? (7, 14, 30 days?)
2. How much storage does Loki use per day for typical home server workload?
3. What is Loki's recommended retention for low-resource deployments?
4. Should different log types have different retention (errors vs info)?
5. How to configure Loki retention policies (compactor settings)?
6. What is the impact of longer retention on query performance?
7. Are there log sampling strategies to reduce volume?
8. How to estimate total log volume for capacity planning?

LOG SOURCE ESTIMATES:
- Pi-hole: DNS queries (potentially thousands per hour)
- Docker container stdout/stderr
- System logs (journald)
- Application-specific logs

SEARCH STRATEGY:
- Search for "Loki retention configuration best practices"
- Look for "home lab log retention recommendations"
- Search for "Loki storage requirements calculator"
- Look for "Docker logging volume estimates"
- Search for "log retention policies for small deployments"

DESIRED OUTPUT:
- Recommended retention period (days) with rationale
- Loki configuration snippet for retention
- Storage capacity estimate (GB per day, GB total)
- Log sampling/filtering recommendations if needed
- Cleanup automation setup
```

---

### MON-02: Grafana Dashboard Templates

**Prompt:**
```
Find and evaluate pre-built Grafana dashboard templates suitable for monitoring the Le Potato home server infrastructure.

MONITORING REQUIREMENTS:

**System Metrics:**
- CPU utilization (per-core and total)
- RAM usage (critical with only 2GB)
- Disk I/O (microSD and external SSD)
- Network throughput (Tailscale, LAN)
- Temperature (if available via sensors)

**Docker Metrics:**
- Container status (running/stopped/restarting)
- Per-container CPU/memory usage
- Container restart count
- Log volume per container

**Service-Specific:**
- Pi-hole: Queries blocked, domains blocked, query types
- Tailscale: Connection status, bandwidth
- Storage: Disk space usage, write volume (SD card wear)

SEARCH QUESTIONS:
1. Are there Grafana dashboards specifically for Pi-hole + Loki?
2. Are there Docker monitoring dashboards that work with Loki?
3. Are there Raspberry Pi/ARM system dashboards (applicable to Le Potato)?
4. What is the Grafana dashboard ID for best Pi-hole dashboard?
5. Are there home server "starter pack" dashboards?
6. Which dashboards work with Netdata as data source?
7. Can dashboards be auto-provisioned in Docker deployment?

SEARCH STRATEGY:
- Browse Grafana dashboard repository (grafana.com/dashboards)
- Search for "Pi-hole Grafana dashboard"
- Search for "Docker Loki Grafana dashboard"
- Look for "Raspberry Pi monitoring dashboard"
- Search for "Netdata Grafana integration"

EVALUATION CRITERIA:
- Low resource overhead (2GB RAM constraint)
- Easy to understand and interpret
- Minimal configuration required
- Active maintenance (recently updated)
- Good community ratings

DESIRED OUTPUT:
- List of 3-5 recommended dashboard IDs with descriptions
- Screenshots or links to examples
- Installation/provisioning instructions
- Required data sources and configuration
- Customization recommendations for this specific setup
```

---

### MON-03: Alert Notification Options

**Prompt:**
```
Research and recommend alerting/notification methods for monitoring the Le Potato home server, focusing on simplicity and reliability.

ALERTING REQUIREMENTS:

**Critical Alerts** (immediate notification):
- Service down: Pi-hole, Tailscale, Docker daemon
- System: High CPU/RAM, disk full, high temperature
- Storage: SD card I/O errors, SSD disconnection

**Warning Alerts** (non-urgent):
- Container restart count excessive
- Log volume high
- Disk space trending toward full

**Notification Constraints:**
- No dedicated monitoring infrastructure (keeping it simple)
- Prefer free or low-cost solutions
- Should work from Grafana or standalone scripts
- Must be reliable (can't miss critical alerts)

NOTIFICATION METHODS TO EVALUATE:

1. **Email** (via Gmail SMTP or similar)
2. **Discord/Slack webhook**
3. **Telegram bot**
4. **ntfy.sh** (push notifications)
5. **Pushover** (mobile push)
6. **SMS** (Twilio, etc. - cost consideration)
7. **Grafana built-in alerting**

RESEARCH QUESTIONS:
1. Which method is most reliable for a home server?
2. Which has easiest setup from Docker containers?
3. What are the ongoing costs (if any)?
4. Can alerts be sent from shell scripts (for non-Grafana monitoring)?
5. What is the alert fatigue risk of each method?
6. Are there ARM/resource constraints for any methods?
7. How to test alerting without generating false alarms?
8. What are best practices for alert suppression/deduplication?

SEARCH STRATEGY:
- Search for "Grafana alerting notification channels"
- Look for "home lab alerting setup"
- Search for "Docker container monitoring alerts"
- Look for "ntfy.sh vs Pushover comparison"
- Search for "self-hosted alerting solutions lightweight"

DESIRED OUTPUT:
- Recommended primary and backup notification methods
- Setup instructions for Grafana integration
- Sample alert rules for critical scenarios
- Cost estimate (if applicable)
- Testing procedure to verify alerts work
```

---

## SYNTHESIS PROMPTS

### SYN-01: Integrated Architecture Recommendations

**Prompt:**
```
After completing individual research prompts HW-01 through MON-03, synthesize findings into integrated architecture recommendations for the Le Potato home server project.

SYNTHESIS OBJECTIVES:
1. Identify any conflicting findings that need reconciliation
2. Highlight showstopper issues that require architecture changes
3. Confirm feasibility of core project goals
4. Prioritize features based on resource constraints
5. Update implementation phase priorities

KEY DECISION POINTS:
- Boot strategy: microSD only, or viable USB boot option?
- Monitoring stack: Full Grafana/Loki, or lightweight alternative needed?
- Storage: ext4 or btrfs for external SSD?
- Networking: Pi-hole networking mode finalized
- Alerting: Which notification method to implement

CONSTRAINT VALIDATION:
- Does everything fit in 2GB RAM with headroom?
- Are USB ports sufficient for planned storage?
- Is power supply adequate?
- Are all critical services ARM-compatible?
- Is 24/7 reliability achievable with this hardware?

OUTPUT FORMAT:
1. **Executive Summary:** Go/No-Go on overall project feasibility
2. **Architecture Revisions:** Changes needed based on research
3. **Risk Assessment:** Updated risk analysis with mitigations
4. **Phase Prioritization:** Reorder implementation phases if needed
5. **Quick Wins:** Identify easiest/highest-value features to implement first
6. **Deferred Features:** What should be postponed due to constraints
7. **Alternative Approaches:** Suggest alternatives for any problematic areas

DESIRED OUTCOME:
A refined project specification incorporating all research findings, with clear go-forward plan and realistic expectations.
```

---

## USAGE INSTRUCTIONS FOR RESEARCH AGENT

### Sequential Thinking Approach

For each prompt above:

1. **Decompose:** Break the research question into sub-questions
2. **Search Strategy:** Execute multiple searches with varying terms
3. **Source Evaluation:** Assess credibility and recency of findings
4. **Synthesis:** Combine information from multiple sources
5. **Practical Application:** Translate findings into actionable recommendations
6. **Confidence Assessment:** Note certainty levels and information gaps

### Search Tool Selection

- **Tavily/Brave:** General web search, product documentation
- **Exa:** Deep technical content, forum discussions, GitHub issues
- **Sequential Thinking:** Complex multi-part questions requiring logical reasoning

### Quality Criteria

- Prioritize recent information (2023-2025)
- Favor official documentation over blog posts
- Look for real-world deployment experiences
- Verify claims across multiple sources
- Note ARM-specific considerations explicitly

---

**End of Research Metaprompts**
