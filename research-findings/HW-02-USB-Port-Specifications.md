# HW-02: USB Port Specifications and Performance

**Research Date:** 2025-10-11
**Board Model:** Libre Computer AML-S905X-CC (Le Potato) 2GB
**SoC:** Amlogic S905X
**Confidence Level:** HIGH

---

## Executive Summary

**CRITICAL LIMITATION IDENTIFIED:** The Le Potato has **USB 2.0 ONLY** - no USB 3.0 ports. This significantly impacts external SSD performance and is a key constraint for your NAS/storage use case.

**Key Findings:**
- 4x USB 2.0 ports (no USB 3.0)
- Real-world SSD throughput: 30-41 MB/s read, 17-35 MB/s write
- Multiple SSDs require powered USB hub (mandatory for stability)
- Known power-related stability issues with external drives
- 24/7 reliability concerns exist but are solvable with proper power management

---

## 1. USB Port Inventory

### Port Configuration
- **Total USB Ports:** 4x USB Type-A
- **USB Version:** USB 2.0 High-Speed (all ports)
- **USB 3.0 Support:** NONE - Not supported by Amlogic S905X SoC

### Bandwidth Architecture
The Le Potato has a unique USB bandwidth allocation:
- **Port 1** (closest to Ethernet jack): Full dedicated bandwidth
- **Ports 2-4** (remaining three): Shared bandwidth pool

This is actually an improvement over Raspberry Pi 3, where all four USB ports share bandwidth with the Ethernet controller.

### SoC USB Controller
- **Chipset:** Integrated Amlogic S905X SoC
- **USB Controllers:** 2x USB 2.0 high-speed controllers (1 OTG + 1 HOST)
- **Maximum Speed:** 480 Mbps theoretical (USB 2.0 specification)
- **Power Management:** Dedicated always-on (AO) power domain

**Source:** Amlogic S905X Datasheet (Rev 0.2), CNX Software technical specifications

---

## 2. Real-World SSD Performance Benchmarks

### USB 2.0 + External SSD Performance

**Measured Throughput:**
- **Sequential Read:** 30-41 MB/s (typical 33-35 MB/s)
- **Sequential Write:** 17-35 MB/s (typical 25-30 MB/s)
- **Performance vs SD Card:** 3.5x improvement (benchmark score: 3,451 vs 939)

### Why USB 2.0 Limits Performance

1. **Half-Duplex Operation:** Data flows in only one direction at a time
2. **Protocol Overhead:** Significant overhead reduces theoretical 480 Mbps to ~35 MB/s effective
3. **Write Verification:** Verify-read after write reduces write speeds by ~50%
4. **Shared Bandwidth:** When using USB hub, 480 Mbps shared across all active devices

### Comparison to USB 3.0 Boards

For reference, the Libre Computer "Renegade" (which has USB 3.0):
- **HDParm Read:** 305.13 MB/s
- **DD Write:** 115 MB/s
- **Performance Multiplier:** ~9x faster reads, ~4x faster writes vs Le Potato

**Verdict:** While SSDs work and provide meaningful improvement over SD cards, you're limited to 30-40 MB/s max due to USB 2.0 bottleneck.

**Sources:** James A. Chambers Le Potato SSD Boot Guide, USB 2.0 specification benchmarks

---

## 3. Multiple SSD Configuration via USB Hub

### Can Multiple SSDs Be Connected?

**Yes, but with critical requirements:**

1. **Powered USB Hub is MANDATORY**
   - Non-negotiable for stability
   - Prevents boot loops and crashes
   - Required for 24/7 operation

2. **Bandwidth Sharing**
   - All devices share 480 Mbps USB 2.0 bandwidth
   - With 3 SSDs active simultaneously: ~10-15 MB/s per drive
   - Realistic shared throughput: 35-40 MB/s total across all drives

3. **Use Case Suitability**
   - **Good for:** Sequential logging, light Docker volumes, Pi-hole (minimal I/O)
   - **Poor for:** Simultaneous heavy read/write, video streaming from multiple sources
   - **Your workload:** "Continuous logging + moderate Docker I/O" - ACCEPTABLE but not optimal

### Transaction Translators

USB hub quality matters:
- **Single-TT Hubs:** All devices share one transaction translator (bottleneck)
- **Multi-TT Hubs:** Separate translators per port (better performance)
- **Recommendation:** Look for Multi-TT hubs for better concurrent performance

**Sources:** USB hub bandwidth allocation documentation, Le Potato forum discussions

---

## 4. USB Power Delivery Specifications

### Per-Port Power Budget

**Standard USB 2.0 Specification:**
- **Per Port:** 500 mA @ 5V = 2.5W maximum
- **Total USB Power:** Limited by board's 5V rail capacity

### Le Potato Power Requirements

**Board Power Draw:**
- **Base Board:** ~1A @ 5V (5W)
- **Per USB Peripheral:** 0.1A to 1A each (0.5W to 5W)
- **Recommended PSU:** Minimum 5V @ 2A, better 5V @ 3A for USB devices
- **Maximum Voltage:** 5.5V (do not exceed - damage risk)

### Power Budget Calculation

**Example for 3x External SSDs:**
- Board: 1A
- SSD 1: 0.5-0.9A (typical 2.5" SSD via USB adapter)
- SSD 2: 0.5-0.9A
- SSD 3: 0.5-0.9A
- **Total Required:** 2.5A - 3.7A @ 5V

**Critical Finding:** High-powered devices like SSDs can draw up to 2A (10W) each - well beyond the SBC's USB power budget.

### SHOWSTOPPER ALERT

**Connecting external storage directly to Le Potato USB ports WILL cause:**
- Boot loops
- System crashes
- Intermittent disconnections
- Unstable 24/7 operation

**Solution:** MUST use powered USB hub with dedicated power supply.

**Sources:** Le Potato official power specifications, Libre Computer Hub forum discussions

---

## 5. Known Stability Issues - 24/7 Operation

### Documented Problems

**Issue #1: Power-Related Crashes**
- **Symptom:** Boot loop when external HDD/SSD connected
- **Cause:** Insufficient power from USB ports
- **Frequency:** Common (multiple forum reports)
- **Status:** Resolved with powered hub

**Issue #2: USB Hub Disconnections**
- **Symptom:** Complete USB hub disconnects after 3-5 minutes
- **Cause:** Power instability or incompatible hub
- **Frequency:** Occasional (specific hub models)
- **Status:** Resolved with quality powered hub + adequate PSU

**Issue #3: Insufficient PSU for Total System**
- **Symptom:** Random crashes during operation
- **Cause:** Total system draw exceeds PSU capacity
- **Frequency:** Common when underpowered
- **Status:** Preventable with proper PSU sizing

### Long-Term Reliability Assessment

**Positive Indicators:**
- Users report successful 24/7 operation for media servers, data logging, torrents
- Board itself is stable when properly powered
- No inherent hardware defects reported for USB subsystem

**Risk Factors:**
- USB 2.0 continuous operation at max bandwidth may cause heat
- Multiple simultaneous I/O operations stress shared bandwidth
- Power fluctuations from inadequate PSU cause cascading failures

### Mitigation Strategy for 24/7 Reliability

1. **Power Supply:** 5V @ 3A minimum, quality brand (CanaKit, official adapters)
2. **USB Hub:** Powered hub with 2.5A+ dedicated USB power
3. **Cooling:** Adequate airflow or heatsink for SoC
4. **Monitoring:** Implement watchdog, temperature monitoring
5. **Conservative Workload:** Don't max out USB bandwidth continuously

**Confidence Level:** MEDIUM-HIGH for 24/7 reliability IF properly configured

**Sources:** Libre Computer Hub forum, James A. Chambers blog, user experience reports

---

## 6. Recommended USB Hub Models

### Top Recommendation: Sabrent Powered Hub

**Model:** Sabrent HB-series (various configurations)
**Specifications:**
- **Dedicated USB Power:** 2.5A @ 5V
- **Ports:** 4-7 data ports (depending on model)
- **USB Version:** USB 3.0 hub (backward compatible with USB 2.0)
- **Power Supply:** Included external adapter
- **Multi-TT:** Yes (better concurrent performance)

**Why Recommended:**
- Specifically mentioned in Le Potato community guides
- Handles NVMe enclosures (highest power draw USB devices)
- Proven track record with SBCs
- Adequate power for multiple SSDs

**Estimated Cost:** $20-30

### Alternative: Sabrent HB-B7C3 (High Port Count)

**Specifications:**
- **Total Ports:** 10 USB Type-A
- **Data Ports:** 7 with full data transfer
- **Charging Ports:** 3 dedicated (higher wattage, no data)
- **Power Supply:** External, high-capacity
- **Use Case:** When you need many devices

### Budget Alternative: Atolla 7-Port Hub

**Specifications:**
- **Ports:** 7x USB 3.0 data + 1x dedicated charging
- **Power Supply:** External adapter included
- **Cost:** Budget-friendly (~$15-20)
- **Note:** Less community validation for Le Potato specifically

### Hub Selection Criteria

**Must Have:**
1. Powered (external PSU included)
2. 2.5A+ dedicated USB power delivery
3. Multi-TT support (if available)
4. USB 3.0 chipset (better power management, backward compatible)

**Nice to Have:**
1. Individual port power switches
2. LED indicators per port
3. Overcurrent protection
4. Metal housing (heat dissipation)

### Recommended USB-to-SATA Adapter

**Model:** StarTech USB 3.1 to 2.5" SATA Adapter
- Proven compatibility with SBCs
- Adequate power delivery to SSDs
- Recommended in James A. Chambers guides
- Works with USB 2.0 (backward compatible)

**Sources:** James A. Chambers Le Potato SSD Boot Guide, Tom's Hardware USB hub reviews

---

## 7. Showstopper Issues & Critical Warnings

### SHOWSTOPPER #1: No USB 3.0

**Impact:** SEVERE for high-throughput NAS use
- Maximum 30-40 MB/s total USB bandwidth
- All external storage shares this bottleneck
- Not suitable for multi-user file server with simultaneous access
- Acceptable for single-user logging and light Docker volumes

**Mitigation:** None - hardware limitation
**Alternative:** Consider Libre Computer "Renegade" or "Sweet Potato" (newer model) if USB 3.0 required

### SHOWSTOPPER #2: Powered Hub Required

**Impact:** CRITICAL - System will not be stable without it
- Direct USB connection to external drives WILL fail
- Not optional for 24/7 operation
- Adds cost ($20-30) and complexity

**Mitigation:** Budget for quality powered USB hub (Sabrent recommended)

### SHOWSTOPPER #3: Power Supply Critical

**Impact:** HIGH - Inadequate PSU causes random failures
- Cannot use phone chargers or low-amperage supplies
- Need 5V @ 3A minimum for board + modest USB load
- Separate powered hub PSU recommended over drawing from board

**Mitigation:** Use quality 5V @ 3A PSU for board + powered hub with dedicated supply

### WARNING #4: Ethernet Bottleneck for NAS

**Impact:** MODERATE
- 100 Mbps Ethernet = 12.5 MB/s maximum network throughput
- Even USB 2.0 SSD at 35 MB/s will be bottlenecked by network
- NAS performance limited by Ethernet, not USB (in this case)

**Silver Lining:** For your use case, Ethernet is the bigger bottleneck than USB 2.0

### WARNING #5: Concurrent I/O Performance

**Impact:** MODERATE to HIGH
- 3 SSDs simultaneously active = ~10-15 MB/s per drive
- Docker container with logging + Pi-hole + file access = potential congestion
- May experience lag during high I/O periods

**Mitigation:**
- Prioritize workloads
- Use eMMC module for OS/critical services
- External SSDs for bulk storage only

---

## 8. Alternative Storage Strategies

### Option A: eMMC Module (Recommended)

**Advantages:**
- Faster than USB 2.0 SSD (50-100 MB/s typical)
- Dedicated bus (no USB sharing)
- More reliable than SD card
- No external power required

**Disadvantages:**
- Limited capacity (8GB, 16GB, 32GB, 64GB options)
- Additional cost ($15-40 depending on size)
- Not hot-swappable

**Use Case:** OS + Docker volumes on eMMC, bulk storage on external USB SSD

### Option B: Single Large SSD

**Advantages:**
- Full 35 MB/s bandwidth (no sharing)
- Simpler configuration
- Lower power draw than multiple drives

**Disadvantages:**
- No redundancy
- Limited capacity expansion

**Use Case:** 1TB or 2TB single SSD for all storage needs

### Option C: NFS/SMB Network Storage

**Advantages:**
- Offload storage to separate NAS device
- Le Potato becomes thin client
- Better scalability

**Disadvantages:**
- Requires separate hardware
- Network dependency
- Additional complexity

---

## 9. Recommendations for Your Use Case

### Your Planned Workload Analysis

**Requirements:**
- 1-3 external SSDs for Docker volumes, logs, NAS storage
- Continuous read/write for logging
- Moderate I/O for Pi-hole and development
- 24/7 reliability required

### Verdict: FEASIBLE WITH CAVEATS

**Configuration Recommendation:**

1. **Storage Architecture:**
   - **eMMC 32GB or 64GB:** Operating system + Docker engine + critical containers
   - **External SSD #1 (500GB-1TB):** Docker volumes + application data
   - **External SSD #2 (1TB-2TB):** Logs + NAS storage (optional)

2. **Hardware Requirements:**
   - **Powered USB Hub:** Sabrent with 2.5A USB power
   - **Power Supply:** 5V @ 3A for Le Potato
   - **USB-to-SATA Adapters:** StarTech or equivalent (2x)
   - **Cooling:** Small heatsink or case with fan

3. **Expected Performance:**
   - **Logging:** Excellent (sequential writes well-suited for USB 2.0)
   - **Pi-hole:** Excellent (minimal I/O, mostly RAM-based)
   - **Docker Containers:** Good (if moderate I/O as stated)
   - **NAS File Access:** Adequate for single user (limited by 100 Mbps Ethernet anyway)

### Performance Expectations

**Realistic Throughput:**
- Single active SSD: 30-35 MB/s
- Two active SSDs: 15-20 MB/s each (shared bandwidth)
- Network file transfer: 10-12 MB/s max (Ethernet bottleneck)

**For Your Workload:**
- Logging rarely exceeds 1-5 MB/s continuous
- Pi-hole uses negligible bandwidth (DNS queries are KB-sized)
- Development I/O typically bursty, not sustained
- **Conclusion:** USB 2.0 bandwidth should be sufficient

### Cost Breakdown

**Minimum Configuration:**
- Le Potato 2GB: $35 (already owned)
- Powered USB Hub: $25
- eMMC 32GB: $20
- 1x USB-to-SATA Adapter: $15
- 1x 1TB SSD: $60
- Power Supply 5V @ 3A: $10
- **Total:** ~$165

**Expanded Configuration (2x SSDs):**
- Add: 1x USB-to-SATA Adapter: $15
- Add: 1x 1TB SSD: $60
- **Total:** ~$240

### Go/No-Go Decision

**GREEN LIGHT IF:**
- You accept USB 2.0 performance limitations
- Your workload is truly "moderate" I/O (not high-throughput)
- You're willing to invest in powered hub + eMMC
- 30-40 MB/s total USB bandwidth is acceptable
- Budget ~$165-240 total

**RED LIGHT IF:**
- You need >50 MB/s sustained throughput
- Multiple containers with heavy concurrent I/O
- Future expansion to high-performance NAS
- Budget constraints prevent powered hub purchase

**Alternative Board Recommendation:**
If USB performance is critical, consider:
- **Libre Computer Renegade:** USB 3.0, ~$50, 305 MB/s read
- **Odroid HC4:** Dual SATA bays, designed for NAS, ~$65
- **Raspberry Pi 4:** USB 3.0, better community support, ~$55

---

## 10. Technical Specifications Summary

| Specification | Value | Notes |
|--------------|-------|-------|
| **USB Ports Total** | 4x USB Type-A | All USB 2.0 |
| **USB 3.0 Ports** | 0 | Not supported by S905X SoC |
| **USB 2.0 Ports** | 4 | 1 dedicated, 3 shared bandwidth |
| **USB Controller** | Amlogic S905X integrated | 1 OTG + 1 HOST |
| **Theoretical Bandwidth** | 480 Mbps (60 MB/s) | Per USB 2.0 spec |
| **Real-World Read** | 30-41 MB/s | Typical 35 MB/s |
| **Real-World Write** | 17-35 MB/s | Typical 25 MB/s |
| **Per-Port Power** | 500 mA @ 5V (2.5W) | USB 2.0 standard |
| **Total USB Power Budget** | ~2.5W usable | Limited by board PSU |
| **SSD Power Draw** | 0.5-2A (2.5-10W) | Exceeds port budget |
| **Powered Hub Required** | YES | Mandatory for stability |
| **Recommended Hub Power** | 2.5A @ 5V minimum | For multiple SSDs |
| **Board Power Supply** | 5V @ 3A recommended | 2A minimum |
| **24/7 Reliability** | Medium-High | With proper power setup |
| **Ethernet Speed** | 100 Mbps (12.5 MB/s) | Bottleneck for NAS use |

---

## 11. Sources & References

### Official Documentation
1. **Amlogic S905X Datasheet (Rev 0.2)** - March 14, 2017
   - URL: https://www.scs.stanford.edu/~zyedidia/docs/amlogic/s905x.pdf
   - USB specifications, SoC architecture

2. **Libre Computer Official Product Page**
   - URL: https://libre.computer/products/aml-s905x-cc/
   - Board specifications, hardware details

### Technical Reviews & Benchmarks
3. **James A. Chambers - Libre Le Potato SSD Boot Guide**
   - URL: https://jamesachambers.com/libre-le-potato-ssd-boot-guide/
   - Real-world SSD performance benchmarks
   - Storage configuration recommendations

4. **James A. Chambers - Le Potato SBC Review**
   - URL: https://jamesachambers.com/libre-computers-le-potato-sbc-review/
   - Comprehensive performance testing
   - Comparison to other SBCs

5. **CNX Software - Amlogic S905X Specifications**
   - URL: https://www.cnx-software.com/2016/01/12/amlogic-s905x-processor-specifications/
   - Detailed SoC specifications
   - USB controller architecture

6. **CNX Software - Does Amlogic S905X Support USB 3.0?**
   - URL: https://www.cnx-software.com/2016/06/24/does-amlogic-s905x-support-usb-3-0/
   - Confirmation of USB 2.0 limitation

### Community Forums & Support
7. **Libre Computer Hub - Boot Loop with Hard Drive**
   - URL: https://hub.libre.computer/t/boot-loop-complete-crash-when-hard-drive-plugged-into-le-potato/1458
   - Power stability issues
   - Community troubleshooting

8. **Libre Computer Hub - 2.5" HDD on Le Potato**
   - URL: https://hub.libre.computer/t/2-5-hdd-on-le-potato/1437
   - External storage configuration
   - Powered hub recommendations

9. **Libre Computer Hub - Powering the Le Potato**
   - URL: https://hub.libre.computer/t/powering-the-le-potato/812
   - Power supply specifications
   - USB power budget discussion

### Technical Standards & Benchmarks
10. **USB 2.0 Specification & Real-World Performance**
    - Multiple sources on USB 2.0 throughput benchmarks
    - Protocol overhead analysis
    - Transaction translator architecture

11. **EverythingUSB - USB 3.2 Speed Comparison**
    - URL: https://www.everythingusb.com/speed.html
    - USB speed comparisons and benchmarks

12. **Tom's Hardware - Best USB Hubs**
    - URL: https://www.tomshardware.com/best-picks/best-usb-hubs
    - USB hub recommendations
    - Powered hub specifications

### Additional References
13. **Electromaker - Le Potato Review**
    - URL: https://www.electromaker.io/blog/article/libre-computer-project-aml-s905x-cc-le-potato-review-24
    - Hardware overview
    - USB port configuration

14. **bret.dk - Libre Computer Le Potato Review**
    - URL: https://bret.dk/libre-computer-le-potato-review-aml-s905x-cc/
    - Real-world usage experience
    - Performance analysis

---

## 12. Conclusion & Final Recommendations

### Critical Findings Summary

1. **USB 2.0 Limitation:** No USB 3.0 - maximum 30-40 MB/s total throughput
2. **Powered Hub Mandatory:** Direct USB storage connections will fail
3. **Power Budget Critical:** 5V @ 3A PSU + 2.5A powered hub required
4. **24/7 Feasibility:** Achievable with proper configuration
5. **Cost Impact:** Additional $65-90 for hub, eMMC, and adapters

### Is Le Potato Suitable for Your Use Case?

**YES - Proceed with Le Potato if:**
- Moderate I/O workload as described (logging, Pi-hole, light Docker)
- Budget flexibility for powered hub and eMMC
- Acceptance of USB 2.0 performance
- Ethernet is primary bottleneck (100 Mbps)

**NO - Consider alternatives if:**
- High-throughput storage required (>50 MB/s)
- Multiple simultaneous heavy I/O operations
- Future expansion to performance NAS
- Budget constraints prevent proper accessories

### Implementation Roadmap

**Phase 1: Foundation**
1. Acquire powered USB hub (Sabrent recommended)
2. Purchase eMMC module (32GB or 64GB)
3. Obtain quality 5V @ 3A power supply

**Phase 2: Storage**
1. Start with single 1TB SSD for Docker + logs
2. Monitor performance and I/O patterns
3. Add second SSD only if needed

**Phase 3: Optimization**
1. Implement I/O monitoring (iotop, iostat)
2. Configure Docker volume locations strategically
3. Set up log rotation to prevent excessive writes

**Phase 4: Reliability**
1. Implement watchdog service
2. Monitor temperatures
3. Set up automated backups
4. Document working configuration

### Final Confidence Assessment

**Overall Confidence Level:** HIGH

- USB specifications: HIGH (well-documented)
- Performance expectations: HIGH (tested by community)
- Stability with proper setup: MEDIUM-HIGH (requires correct configuration)
- Suitability for stated use case: MEDIUM-HIGH (workload dependent)

**Primary Risk:** USB 2.0 bandwidth constraint if workload exceeds expectations

**Primary Mitigation:** Start with conservative workload, monitor performance, scale cautiously

---

**Research Completed By:** Claude (Anthropic)
**Research Date:** 2025-10-11
**Document Version:** 1.0
**Next Steps:** Review findings, make Go/No-Go decision, proceed to hardware acquisition if approved
