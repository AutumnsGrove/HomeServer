# Le Potato Home Server - Research Planning Package

**Generated:** October 11, 2025  
**Purpose:** Pre-implementation research planning for Le Potato home server project  
**Status:** Planning Mode - No research executed yet

---

## 📦 What's Included

This package contains three comprehensive documents to guide the research phase of your Le Potato home server project:

### 1. **lepotato-research-prompts.md** (Main Document)
The core research metaprompts organized by category:

- **Hardware Research** (4 prompts): USB boot, USB specs, power, thermal management
- **Software Research** (5 prompts): Pi-hole networking, Grafana/Loki, Docker, Claude Code, happy.engineering
- **Storage Research** (4 prompts): Filesystem choice, log relocation, Docker volumes, backups
- **Monitoring Research** (3 prompts): Log retention, dashboards, alerting
- **Synthesis** (1 prompt): Integrating all findings

**Total: 17 research prompts** designed for execution by a research agent with sequential thinking and web search capabilities.

### 2. **research-execution-guide.md** (Strategy Document)
Practical guidance on executing the research:

- **Priority ordering** of prompts (Critical → High → Medium → Low)
- **Phase-based execution** strategy with time estimates
- **Decision gates** and red flags to watch for
- **Priority adjustments** based on findings
- **Success metrics** for completed research

### 3. **research-findings-template.md** (Documentation Template)
Standardized template for documenting findings from each prompt:

- Executive summary format
- Detailed analysis structure
- Implementation guidance sections
- Risk assessment framework
- Source citation format
- Confidence level rating system

---

## 🎯 Project Context

### Hardware
- **Board:** Libre Computer AML-S905X-CC (Le Potato)
- **Model:** AML-S905X-CC-2GB
- **CPU:** Quad-core ARM Cortex-A53 @ 1.5GHz (64-bit)
- **RAM:** 2GB DDR3
- **Boot:** 256GB SanDisk microSD card
- **Storage:** External SSD (USB 3.0, planning 2TB)
- **Network:** Tailscale VPN + local LAN

### Primary Goals
1. **Reliable 24/7 home server** with self-healing capabilities
2. **Remote access** via Tailscale VPN (no local interaction needed)
3. **Network-wide ad blocking** with Pi-hole
4. **Development environment** with Claude Code + happy.engineering
5. **NAS capabilities** with external SSD storage
6. **Comprehensive monitoring** with Grafana + Loki
7. **Minimal SD card wear** for longevity

### Key Constraints
- Only 2GB RAM (must budget carefully)
- ARM64 architecture (software compatibility questions)
- USB-attached storage (performance and reliability concerns)
- microSD boot with longevity concerns (1-3 year lifespan)
- Home server use case (simplicity and low maintenance preferred)

---

## 🚀 How to Use This Package

### Step 1: Review the Project Spec
Read the original `le-potato-server-spec.md` to understand the full context and goals.

### Step 2: Understand the Execution Strategy
Open `research-execution-guide.md` and review:
- The priority ordering of prompts
- The phase-based approach
- The decision gates and red flags
- The time estimates for each phase

### Step 3: Execute Research Prompts
Use `lepotato-research-prompts.md` as your guide:

**For each prompt:**
1. Read the full prompt carefully (don't skim!)
2. Note the specific hardware/software context
3. Understand the research questions
4. Follow the suggested search strategy
5. Aim for the desired output format

**Recommended tool usage:**
- **Sequential Thinking:** For complex multi-part questions
- **Tavily/Brave Search:** For general web searches, documentation
- **Exa/Perplexity:** For deep technical content, GitHub issues, forums

### Step 4: Document Findings
For each completed prompt:
1. Copy the `research-findings-template.md`
2. Rename it (e.g., `HW-01-USB-Boot-Findings.md`)
3. Fill in all sections thoroughly
4. Assign a confidence level (High/Medium/Low)
5. Provide clear recommendations

### Step 5: Synthesize Results
After completing all 16 specific prompts:
1. Execute the **SYN-01** synthesis prompt
2. Look for conflicts or showstoppers
3. Update architecture decisions
4. Revise implementation phase priorities
5. Create updated project specification v2.0

---

## 📊 Critical Path Analysis

### Must Answer First (Showstopper Potential)
1. **HW-02: USB Port Specifications** - Can storage plan work?
2. **HW-03: Power Supply Requirements** - Is power adequate?
3. **SW-02: Grafana/Loki Resources** - Will monitoring fit in 2GB RAM?
4. **SW-04: Claude Code ARM Compatibility** - Is dev environment viable?

**⚠️ DECISION GATE:** If any show showstoppers, pause and redesign before proceeding with remaining research.

### High Impact on Architecture
- **SW-01: Pi-hole Docker Networking** - Core service configuration
- **HW-01: USB Boot Capability** - Long-term storage strategy
- **ST-01: ext4 vs btrfs** - Storage architecture foundation
- **HW-04: Thermal Management** - 24/7 reliability requirement

### Lower Priority (But Still Important)
- **SW-05: happy.engineering** - Nice-to-have dev tool
- **ST-02 through ST-04** - Storage optimizations
- **MON-01 through MON-03** - Monitoring refinements

---

## ⚡ Quick Start for Research Agent

If you're a research agent about to execute these prompts:

```
1. START with Critical Path prompts (HW-02, HW-03, SW-02, SW-04)
2. STOP at first decision gate to assess feasibility
3. IF feasible → CONTINUE with High Priority prompts
4. IF showstoppers → FLAG for human review and architecture redesign
5. DOCUMENT findings using the template (be thorough!)
6. SYNTHESIZE at the end (SYN-01 prompt)
7. DELIVER updated project specification with confidence levels
```

---

## 🎨 Research Quality Guidelines

### Good Research Practices
- ✅ Multiple sources for critical claims (verify, don't trust blindly)
- ✅ Prioritize recent information (2023-2025)
- ✅ Favor official documentation over blog posts
- ✅ Look for real-world deployment experiences
- ✅ Note ARM-specific considerations explicitly
- ✅ Document confidence levels honestly
- ✅ Flag uncertainties and conflicting information

### Red Flags
- ❌ Single source for critical decisions
- ❌ Outdated information (pre-2022 unless still relevant)
- ❌ Generic answers not specific to ARM/SBC context
- ❌ Unverified claims or speculation
- ❌ Ignoring resource constraints (2GB RAM!)
- ❌ Not considering 24/7 reliability requirements

---

## 📋 Expected Deliverables

After completing all research, you should have:

1. **17 findings documents** (one per prompt, using template)
2. **Updated project specification v2.0** (incorporating research)
3. **Architecture decision records** (why choices were made)
4. **Risk assessment matrix** (updated with research findings)
5. **Implementation checklist** (prioritized and sequenced)
6. **Shopping list** (any additional hardware needed)

---

## 🔄 Iteration & Refinement

Research is iterative. If findings suggest major changes:

1. **Document the conflict** clearly
2. **Propose alternatives** with pros/cons
3. **Seek human input** on strategic decisions
4. **Update related prompts** that depend on the change
5. **Re-research** affected areas if needed

---

## 💡 Tips for Success

### Time Management
- Estimated total research time: **10-15 hours**
- Don't get stuck on any single prompt for more than 1-2 hours
- If uncertain, document the uncertainty and move on
- You can always circle back for deeper dives

### Search Strategy
- Start broad, then narrow based on initial findings
- Use specific model numbers (AML-S905X-CC, not just "Le Potato")
- Include year in searches for recent info ("2024", "2025")
- Check multiple forums: Reddit, forum.librecomputer.io, Armbian forums
- Look for GitHub issues on relevant projects

### Avoiding Rabbit Holes
- Stay focused on the specific research questions
- Don't research tangential topics (however interesting!)
- Remember: good enough > perfect
- High confidence on critical path, medium confidence acceptable elsewhere

### When to Ask for Help
- When findings conflict and you can't resolve
- When you find potential showstoppers
- When confidence remains low after thorough search
- When recommendations have significant cost/effort implications

---

## 📚 Key Resources to Bookmark

### Official Documentation
- Libre Computer: https://libre.computer/
- Libre Computer Docs: https://hub.librecomputer.io/
- Ubuntu ARM: https://ubuntu.com/download/server/arm

### Community Forums
- Libre Computer Forum: https://forum.librecomputer.io/
- Armbian Forum: https://forum.armbian.com/
- Reddit r/selfhosted: https://reddit.com/r/selfhosted

### Technical References
- Pi-hole Docker: https://github.com/pi-hole/docker-pi-hole
- Tailscale Docs: https://tailscale.com/kb/
- Grafana Loki: https://grafana.com/docs/loki/
- Docker ARM: https://docs.docker.com/

---

## 🎯 Success Criteria

Research phase is complete when:

- ✅ All 17 prompts have documented findings
- ✅ All critical path questions answered with high confidence
- ✅ No unresolved showstoppers (or alternatives identified)
- ✅ Resource budget validated (RAM, storage, power)
- ✅ Architecture decisions documented with rationale
- ✅ Implementation phases prioritized based on findings
- ✅ Project specification updated to v2.0
- ✅ Ready to begin Phase 1 implementation

---

## 🚦 Next Steps After Research

Once research is complete:

1. **Review** all findings with project stakeholder (you!)
2. **Finalize** architecture decisions and document rationale
3. **Order** any additional hardware identified
4. **Prepare** implementation environment
5. **Begin** Phase 1: Base System Setup
6. **Reference** research findings during implementation

---

## 📝 Notes

- These prompts are **living documents** - update if you discover better questions
- **Version control** your findings - they're valuable reference material
- **Share findings** with the community if you discover novel solutions
- **Keep a lab notebook** - document what works and what doesn't
- **Celebrate small wins** - each answered question is progress!

---

## 🙏 Acknowledgments

This research planning package is based on:
- The detailed project specification in `le-potato-server-spec.md`
- Product specifications from the Amazon listing for AML-S905X-CC
- Best practices from the home lab and self-hosting communities
- Lessons learned from similar Raspberry Pi/SBC server projects

---

## 📞 Questions or Issues?

If you encounter:
- **Unclear prompts:** Refine the prompt and document the improvement
- **Missing context:** Add it to the prompt and note what was missing
- **New questions:** Add them to the "Open Questions" in findings
- **Showstoppers:** Document clearly and seek input before proceeding

---

**Happy researching! May your searches be fruitful and your findings clear. 🔍🎉**

---

**End of README**
