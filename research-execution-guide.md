# Le Potato Research Execution Guide

## Quick Reference: Research Prompt Priority

### CRITICAL PATH (Execute First)
These directly impact whether the project is feasible:

1. **HW-02: USB Port Specifications** - Determines if storage plan is viable
2. **HW-03: Power Supply Requirements** - Validates power architecture
3. **SW-02: Grafana/Loki Resources** - 2GB RAM constraint validation
4. **SW-04: Claude Code ARM Compatibility** - Core dev environment dependency

**DECISION GATE:** If any of these show showstoppers, pause and redesign before proceeding.

---

### HIGH PRIORITY (Execute Second)
Critical for core functionality but have workarounds:

5. **SW-01: Pi-hole Docker Networking** - Primary service architecture
6. **HW-01: USB Boot Capability** - Long-term SD card strategy
7. **ST-01: Filesystem Choice (ext4 vs btrfs)** - Storage architecture
8. **HW-04: Thermal Management** - 24/7 reliability concern

---

### MEDIUM PRIORITY (Execute Third)
Important for full feature set:

9. **SW-05: happy.engineering ARM Support** - Dev environment enhancement
10. **ST-02: /var/log Relocation** - SD card longevity optimization
11. **MON-01: Log Retention Requirements** - Storage planning
12. **MON-02: Grafana Dashboard Templates** - Monitoring implementation

---

### LOWER PRIORITY (Execute Fourth)
Refinements and nice-to-haves:

13. **SW-03: Docker Storage Driver** - Optimization (default probably fine)
14. **ST-03: Docker Volume Driver Options** - Management approach
15. **ST-04: Backup Solution Selection** - Post-deployment concern
16. **MON-03: Alert Notification Options** - Enhancement feature

---

### SYNTHESIS (Execute Last)

17. **SYN-01: Integrated Architecture Recommendations** - Combine all findings

---

## Execution Strategy

### Phase 1: Feasibility Check (Prompts 1-4)
**Goal:** Confirm project is viable with current hardware  
**Time estimate:** 2-3 hours  
**Stop condition:** If major showstoppers found, redesign required

**Key Questions Answered:**
- Can the storage plan work with USB limitations?
- Is power supply adequate?
- Will monitoring fit in 2GB RAM?
- Is development environment possible?

### Phase 2: Core Architecture (Prompts 5-8)
**Goal:** Finalize critical design decisions  
**Time estimate:** 2-3 hours  
**Output:** Updated architecture diagrams and decisions

**Key Questions Answered:**
- How to configure Pi-hole networking?
- Should we pursue USB boot?
- ext4 or btrfs for storage?
- What cooling is needed?

### Phase 3: Implementation Details (Prompts 9-12)
**Goal:** Gather specific configuration guidance  
**Time estimate:** 2-3 hours  
**Output:** Configuration files and procedures

### Phase 4: Refinements (Prompts 13-16)
**Goal:** Optimize and enhance  
**Time estimate:** 1-2 hours  
**Output:** Best practices and enhancement options

### Phase 5: Integration (Prompt 17)
**Goal:** Synthesize all findings into unified plan  
**Time estimate:** 1-2 hours  
**Output:** Updated project specification v2.0

---

## Research Agent Execution Template

For each prompt, use this workflow:

```
1. READ the full prompt carefully
2. IDENTIFY the 3-5 most critical questions
3. PLAN search strategy (which tools, which terms)
4. EXECUTE 5-10 searches with varying approaches
5. EVALUATE source quality and consistency
6. SYNTHESIZE findings into clear recommendations
7. FLAG any uncertainties or conflicting information
8. DOCUMENT confidence level (High/Medium/Low)
```

---

## Red Flags to Watch For

During research, immediately escalate if you find:

- **Hardware incompatibilities** that can't be worked around
- **Software that doesn't support ARM64** with no alternatives
- **Resource requirements exceeding 1.5GB RAM** (Pi-hole must always run)
- **Known stability issues** with 24/7 operation
- **USB performance bottlenecks** below 100MB/s sustained
- **Power requirements** exceeding reasonable PSU specs
- **Thermal throttling** under normal workload

---

## Output Format for Each Prompt

Structure findings as:

### Summary
2-3 sentence executive summary with recommendation

### Findings
Bulleted key discoveries with sources

### Recommendation
Specific actionable guidance

### Configuration
Sample configs, commands, or code snippets

### Gotchas
Known issues, limitations, or workarounds

### Confidence Level
- High: Multiple authoritative sources agree
- Medium: Limited sources or some uncertainty
- Low: Conflicting information or no clear answer

### Next Steps
Actions needed based on findings

---

## Priority Adjustments Based on Findings

**IF USB boot is NOT viable:**
- Elevate ST-02 (/var/log relocation) to HIGH priority
- Add research on SD card wear monitoring tools
- Plan for more frequent SD card replacement

**IF Grafana/Loki too resource-heavy:**
- Research lightweight alternatives (Dozzle, Portainer logs)
- Consider external monitoring VM
- Simplify to essential logging only

**IF Claude Code not ARM-compatible:**
- Research alternative development approaches
- Consider web-based IDEs (code-server, Theia)
- May need to use laptop as dev environment

**IF Pi-hole + Tailscale networking complex:**
- Research alternative DNS filtering (AdGuard Home)
- Consider simplified architecture
- May need to separate DNS and VPN concerns

---

## Documentation Template

As you complete each research prompt, create a findings document:

```markdown
# [HW-XX / SW-XX / ST-XX / MON-XX]: [Topic Name]

**Research Date:** [Date]  
**Confidence Level:** [High/Medium/Low]  
**Status:** [Go / No-Go / Go-with-Modifications]

## Summary
[2-3 sentence conclusion]

## Key Findings
1. [Finding with source]
2. [Finding with source]
3. [Finding with source]

## Recommendation
[Clear actionable guidance]

## Implementation Notes
[Specific steps, configs, or commands]

## Risks & Mitigations
[Known issues and how to address them]

## Sources
- [Primary source 1]
- [Primary source 2]
- [Community discussion]

## Open Questions
[Anything still uncertain]
```

---

## Success Metrics

After completing all research:

- ✅ All critical questions have High or Medium confidence answers
- ✅ No showstopper issues without viable workarounds
- ✅ Resource budget validated (CPU, RAM, storage, power)
- ✅ All required software confirmed ARM-compatible
- ✅ Network architecture finalized and documented
- ✅ Monitoring approach confirmed feasible
- ✅ Backup strategy selected
- ✅ Project specification updated with research findings

---

## Next Step After Research

Once all prompts are completed and synthesized:

1. Update project specification to v2.0
2. Create detailed implementation checklist
3. Order any additional hardware identified
4. Begin Phase 1 implementation (Base System Setup)
5. Maintain research notes for troubleshooting reference

---

**Ready to begin research when you are!**
