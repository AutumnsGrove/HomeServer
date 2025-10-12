# Le Potato Research - Quick Reference Card

**Hardware:** Le Potato AML-S905X-CC | 2GB RAM | ARM Cortex-A53 @ 1.5GHz  
**Goal:** Research feasibility and architecture decisions for 24/7 home server  
**Estimated Time:** 10-15 hours total

---

## 🎯 Research Prompts at a Glance

### CRITICAL PATH - DO FIRST ⚡
| ID | Topic | Why Critical | Est. Time |
|---|---|---|---|
| HW-02 | USB Specs | Storage plan viability | 45 min |
| HW-03 | Power Requirements | Power budget validation | 30 min |
| SW-02 | Grafana/Loki Resources | 2GB RAM constraint | 60 min |
| SW-04 | Claude Code ARM | Dev environment core | 45 min |

**⚠️ STOP HERE if showstoppers found - redesign before continuing!**

---

### HIGH PRIORITY 🔥
| ID | Topic | Est. Time |
|---|---|---|
| SW-01 | Pi-hole Docker Networking | 45 min |
| HW-01 | USB Boot Capability | 30 min |
| ST-01 | ext4 vs btrfs | 45 min |
| HW-04 | Thermal Management | 30 min |

---

### MEDIUM PRIORITY 📊
| ID | Topic | Est. Time |
|---|---|---|
| SW-05 | happy.engineering ARM | 30 min |
| ST-02 | /var/log Relocation | 30 min |
| MON-01 | Log Retention | 20 min |
| MON-02 | Grafana Dashboards | 30 min |

---

### LOWER PRIORITY 🔧
| ID | Topic | Est. Time |
|---|---|---|
| SW-03 | Docker Storage Driver | 20 min |
| ST-03 | Docker Volume Drivers | 20 min |
| ST-04 | Backup Solution | 45 min |
| MON-03 | Alert Notifications | 30 min |

---

### SYNTHESIS 🔄
| ID | Topic | Est. Time |
|---|---|---|
| SYN-01 | Integrated Recommendations | 90 min |

---

## 📝 Research Workflow

```
1. READ full prompt (don't skim!)
   ├─ Understand hardware context
   ├─ Note critical questions
   └─ Review desired output format

2. PLAN search strategy
   ├─ Identify 3-5 key search terms
   ├─ Choose appropriate tools
   └─ Set 1-2 hour time limit

3. EXECUTE searches
   ├─ Start broad, then narrow
   ├─ 5-10 searches minimum
   ├─ Verify across multiple sources
   └─ Note conflicting information

4. SYNTHESIZE findings
   ├─ Answer original questions
   ├─ Provide clear recommendation
   └─ Document confidence level

5. DOCUMENT results
   ├─ Use findings template
   ├─ Include sources
   └─ Note open questions
```

---

## 🔍 Search Strategy Tips

### Model-Specific Searches
✅ Use exact model: "AML-S905X-CC"  
✅ Include "2GB" for RAM-specific info  
✅ Add "ARM64" or "aarch64" for architecture

### Effective Search Terms
```
"AML-S905X-CC USB boot"
"Le Potato 24/7 server"
"Pi-hole ARM Docker networking"
"Grafana Loki 2GB RAM"
"Raspberry Pi [topic]" (similar hardware)
```

### Date Filtering
- Add "2024" or "2025" for recent info
- For hardware: any date OK if specs haven't changed
- For software: prioritize last 2 years

### Where to Search
1. Official documentation (highest priority)
2. GitHub issues (real-world problems/solutions)
3. Reddit r/selfhosted, r/homelab
4. Forum.librecomputer.io
5. Stack Overflow / Server Fault

---

## 🚩 Red Flags - Escalate Immediately

| Finding | Implication | Action |
|---------|-------------|--------|
| USB 3.0 < 100MB/s sustained | Storage bottleneck | Consider alternatives |
| RAM requirement > 1.5GB | Won't fit with Pi-hole | Scale down or skip |
| No ARM64 binary available | Software incompatible | Find alternative |
| Known stability issues | 24/7 operation at risk | Need workaround |
| Power > 5V 3A | PSU inadequate | Need bigger PSU |
| Thermal throttling < 50°C | Cooling insufficient | Add active cooling |

---

## 📊 Confidence Levels

### 🟢 HIGH CONFIDENCE
- Multiple official sources agree
- Recent information (last 2 years)
- Real-world deployment examples
- Clear consensus in community

### 🟡 MEDIUM CONFIDENCE
- Limited sources (1-2)
- Some conflicting information
- Older information but likely still valid
- Theoretical but reasonable

### 🔴 LOW CONFIDENCE
- Single unverified source
- Significant conflicts between sources
- No recent information found
- Requires hands-on testing

**Rule:** Critical path needs HIGH confidence. Others can be MEDIUM.

---

## 📋 Output Checklist (Per Prompt)

- [ ] Executive summary (2-3 sentences)
- [ ] Key findings with sources (3-5 minimum)
- [ ] Clear recommendation
- [ ] Implementation guidance (commands/configs)
- [ ] Known issues and workarounds
- [ ] Confidence level assigned
- [ ] Sources documented (URLs)
- [ ] Open questions noted

---

## 💻 Hardware Context - Quick Reference

```
Board:    Le Potato AML-S905X-CC-2GB
SoC:      Amlogic S905X (ARM Cortex-A53)
CPU:      4 cores @ 1.5GHz (64-bit)
RAM:      2GB DDR3 (CRITICAL CONSTRAINT)
GPU:      Mali-450 @ 750MHz
Boot:     microSD (256GB SanDisk)
Storage:  External SSD via USB 3.0
Network:  Gigabit Ethernet
Power:    5V DC (amperage TBD in research)
Cooling:  Small fan + case (current setup)
OS:       Ubuntu Server 22.04 LTS ARM64
```

---

## 🎯 Success Metrics

After all research:

- [ ] All critical path: HIGH confidence
- [ ] No unresolved showstoppers
- [ ] RAM budget: < 1.8GB allocated (0.2GB free)
- [ ] CPU load: < 50% average expected
- [ ] Storage: Clear understanding of performance
- [ ] Power: PSU spec determined
- [ ] Network: Pi-hole architecture finalized
- [ ] All core services: ARM compatibility confirmed

---

## ⏱️ Time Allocation

| Phase | Time | Prompts |
|-------|------|---------|
| Critical Path | 3 hours | HW-02, HW-03, SW-02, SW-04 |
| High Priority | 3 hours | SW-01, HW-01, ST-01, HW-04 |
| Medium Priority | 2 hours | SW-05, ST-02, MON-01, MON-02 |
| Lower Priority | 2 hours | SW-03, ST-03, ST-04, MON-03 |
| Synthesis | 1.5 hours | SYN-01 |
| **TOTAL** | **11.5 hours** | |

*Add 20% buffer = 14 hours realistic estimate*

---

## 🔄 When to Stop & Pivot

Stop researching and escalate if:

- ⏰ Spent > 2 hours on one prompt with low confidence
- 🚫 Found definitive showstopper with no workaround
- ❓ Need hands-on testing to answer question
- 🔀 Findings suggest major architecture change
- 💰 Findings suggest significant cost increase

---

## 📞 Questions to Ask If Stuck

1. Am I searching for the right thing?
2. Have I checked official documentation?
3. Have I searched GitHub issues?
4. Have I looked at similar hardware (Raspberry Pi)?
5. Is this question actually answerable without testing?
6. Can I find an expert blog post or video?
7. Should I ask the community directly?

---

## 🎨 Quality Over Speed

**Remember:**
- ⚡ Fast + wrong = waste of time
- 🐌 Slow + thorough = solid foundation
- 🎯 Medium confidence + documented = acceptable
- 🚫 High confidence + false = disaster

**Better to say "I don't know, needs testing" than to guess!**

---

## 📚 Resource URLs

| Resource | URL |
|----------|-----|
| Libre Computer | https://libre.computer |
| Forum | https://forum.librecomputer.io |
| Pi-hole Docker | https://github.com/pi-hole/docker-pi-hole |
| Tailscale | https://tailscale.com/kb |
| Grafana Loki | https://grafana.com/docs/loki |
| Ubuntu ARM | https://ubuntu.com/download/server/arm |

---

## 🎉 Motivation

**You've got this!** 

Each answered question moves the project forward. Each documented finding saves time during implementation. Each source cited builds confidence in the plan.

Research now = fewer surprises later.

---

**Print or bookmark this card for quick reference during research!**
