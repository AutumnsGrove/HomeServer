# Research Findings Template

Copy this template for each research prompt you complete.

---

# [PROMPT-ID]: [Research Topic]

**Prompt ID:** HW-01 / HW-02 / SW-01 / etc.  
**Research Date:** YYYY-MM-DD  
**Researcher:** [Name/Agent ID]  
**Time Spent:** [Hours]  
**Confidence Level:** 🟢 High / 🟡 Medium / 🔴 Low  
**Status:** ✅ Go / ⚠️ Go-with-Modifications / ❌ No-Go

---

## Executive Summary

[2-3 sentences answering the core question with the recommendation]

Example:
> The Le Potato AML-S905X-CC does NOT support native USB boot as of October 2025. However, SD card longevity can be significantly extended by relocating high-write workloads to external SSD. Recommend proceeding with SD boot + aggressive I/O offloading strategy.

---

## Key Findings

### Finding 1: [Topic]
**Source:** [URL or documentation reference]  
**Reliability:** [Official docs / Community consensus / Single report]

[Detailed finding]

### Finding 2: [Topic]
**Source:** [URL or documentation reference]  
**Reliability:** [Official docs / Community consensus / Single report]

[Detailed finding]

### Finding 3: [Topic]
**Source:** [URL or documentation reference]  
**Reliability:** [Official docs / Community consensus / Single report]

[Detailed finding]

[Continue as needed...]

---

## Detailed Analysis

### Context
[Relevant background information or technical details]

### Methodology
[How the research was conducted: search terms used, sources consulted, tests performed]

### Results
[Detailed results, benchmarks, specifications found]

### Interpretation
[What the findings mean for the project]

---

## Recommendation

### Primary Recommendation
[Clear, actionable main recommendation]

**Rationale:**
[Why this is the recommended approach]

### Alternative Options
1. **[Alternative 1]:** [Brief description and when to consider]
2. **[Alternative 2]:** [Brief description and when to consider]

### If Recommendation Not Followed
[Consequences or what to consider instead]

---

## Implementation Guidance

### Prerequisites
- [Required software/hardware]
- [Dependencies]
- [Prior steps that must be completed]

### Step-by-Step Procedure

```bash
# Step 1: [Description]
[command or action]

# Step 2: [Description]
[command or action]

# Step 3: [Description]
[command or action]
```

### Configuration Files

**File:** `/path/to/config/file`
```
[configuration snippet]
```

**File:** `/another/config/file`
```
[configuration snippet]
```

### Verification

```bash
# How to verify the configuration works
[test commands]
```

Expected output:
```
[what success looks like]
```

---

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk description] | Low/Med/High | Low/Med/High | [How to address] |
| [Risk description] | Low/Med/High | Low/Med/High | [How to address] |

---

## Resource Requirements

- **RAM:** [Memory required or overhead]
- **CPU:** [Processing requirements]
- **Storage:** [Disk space needed]
- **Network:** [Bandwidth considerations]
- **Power:** [Power draw if relevant]

---

## Known Issues & Workarounds

### Issue 1: [Description]
**Symptoms:** [How it manifests]  
**Workaround:** [How to resolve]  
**Source:** [Where this is documented]

### Issue 2: [Description]
**Symptoms:** [How it manifests]  
**Workaround:** [How to resolve]  
**Source:** [Where this is documented]

---

## Performance Characteristics

[If applicable: benchmarks, latency, throughput, resource usage under load]

### Expected Performance
- [Metric 1]: [Value]
- [Metric 2]: [Value]
- [Metric 3]: [Value]

### Performance Comparison
[If comparing options, show side-by-side data]

---

## Architecture Impact

### Changes Required to Project Spec
- [Specific modification 1]
- [Specific modification 2]

### Dependencies Affected
- [What else needs to change based on this finding]

### Phase Priority Adjustments
- [If this finding changes implementation order]

---

## Sources & References

### Primary Sources (Official Documentation)
1. [Title] - [URL]
2. [Title] - [URL]

### Secondary Sources (Community/Forums)
1. [Title] - [URL] - [Date published]
2. [Title] - [URL] - [Date published]

### Code/Configuration Examples
1. [GitHub repo or example] - [URL]
2. [GitHub repo or example] - [URL]

### Related Discussions
1. [Reddit/Forum thread] - [URL] - [Summary]
2. [Reddit/Forum thread] - [URL] - [Summary]

---

## Open Questions & Uncertainties

### Unresolved Questions
1. [Question that still needs research]
2. [Aspect that requires hands-on testing]

### Low-Confidence Areas
[Aspects of the findings where confidence is lower and why]

### Recommended Follow-Up Research
[Additional research that would improve confidence]

---

## Testing & Validation Plan

### Pre-Implementation Testing
[What to test before committing to this approach]

### Post-Implementation Testing
[How to verify it works in production]

### Success Criteria
- ✅ [Measurable success criterion 1]
- ✅ [Measurable success criterion 2]
- ✅ [Measurable success criterion 3]

### Rollback Plan
[How to undo this if it doesn't work]

---

## Confidence Level Justification

**Rating:** 🟢 High / 🟡 Medium / 🔴 Low

**Justification:**
[Explain why this confidence level was assigned]

**Factors Increasing Confidence:**
- [Factor 1]
- [Factor 2]

**Factors Decreasing Confidence:**
- [Factor 1]
- [Factor 2]

---

## Tags & Categories

`#hardware` `#software` `#storage` `#monitoring` `#arm64` `#docker` `#critical-path` `#nice-to-have`

---

## Changelog

| Date | Change | Reason |
|------|--------|--------|
| YYYY-MM-DD | Initial research | First findings |
| YYYY-MM-DD | Updated with additional source | New information found |

---

## Reviewer Notes

[Space for peer review comments or validation]

---

**End of Findings Document**
