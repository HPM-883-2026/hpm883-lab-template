# Quick Decision Guide: Lab Solution Release

**TL;DR**: Use **simple manual push** for HPM 883. Takes 2 min/lab, zero infrastructure.

---

## Decision Matrix

| Factor | Your Situation | Recommendation |
|--------|---------------|----------------|
| **# of labs/semester** | 12 | Manual push (24 min total) |
| **Timing precision needed** | "After class ends" (~flexible) | Manual push (run when ready) |
| **Tech comfort** | High (git, GitHub) | Any approach works |
| **Debugging time available** | Low (mid-semester is busy) | Manual push (zero debugging) |
| **Infrastructure preference** | Minimal | Manual push |

---

## The Numbers

| Approach | Setup Time | Per-Lab Time | Semester Total | Reliability | Debugging |
|----------|-----------|--------------|----------------|-------------|-----------|
| **Manual push** | 5 min | 2 min | **29 min** | 100% | None |
| Protected branch unlock | 10 min | 3 min | 46 min | 95% | Rare |
| GitHub Actions | 60-120 min | 0 min* | 60-120 min | 70-85% | High |
| Separate repo | 20 min | 5 min | 80 min | 90% | Medium |

\* When working. Debugging time not included in GitHub Actions total.

---

## When Each Approach Makes Sense

### Manual Push (RECOMMENDED for HPM 883)
**Best for**:
- ✅ 5-20 labs/semester
- ✅ Instructor has 2 minutes after class
- ✅ Value reliability over automation
- ✅ Want zero infrastructure complexity

**Not ideal for**:
- ❌ 50+ labs/semester (automation saves time)
- ❌ Multiple instructors need coordinated releases
- ❌ Instructor unavailable after class (traveling, etc.)

---

### GitHub Actions Scheduled Workflow
**Best for**:
- ✅ 50+ labs/semester (setup cost amortizes)
- ✅ Exact timing not critical (±15-30 min delays acceptable)
- ✅ Comfortable debugging YAML workflows
- ✅ Want "set and forget" automation

**Not ideal for**:
- ❌ Small number of labs (setup exceeds benefit)
- ❌ Need precise timing (workflows can delay during peak load)
- ❌ Limited mid-semester debugging time

---

### Protected Branch Unlock
**Best for**:
- ✅ Want solutions isolated pre-release
- ✅ Comfortable with GitHub settings UI
- ✅ Don't mind weekly admin tasks

**Not ideal for**:
- ❌ Forgetful (easy to forget unlocking step)
- ❌ Want streamlined workflow
- ❌ Need automatic reminders

---

### Separate Solutions Repo
**Best for**:
- ✅ Solutions contain sensitive info
- ✅ Need granular access control (some students get solutions, others don't)
- ✅ Want clear separation of concerns

**Not ideal for**:
- ❌ Want integrated student experience (solutions next to their work)
- ❌ Simple "all students get solutions after class" scenario
- ❌ Want minimal administrative overhead

---

## The Recommended Setup: Manual Push

### Why It Wins for HPM 883

**Total time investment**:
- Setup: 5 minutes (one-time)
- Execution: 12 labs × 2 min = 24 minutes
- **Total: 29 minutes for entire semester**

**Compare to alternatives**:
- GitHub Actions: 60-120 min setup + unknown debugging hours
- Separate repo: 20 min setup + 60 min execution = 80 min
- Protected branch: 10 min setup + 36 min execution = 46 min

**Risk assessment**:
- **Manual push**: Zero risk (100% reliability, predictable effort)
- **GitHub Actions**: Medium-high risk (workflow delays, debugging mid-semester)
- **Protected branch**: Low risk (might forget to unlock)
- **Separate repo**: Low risk (admin overhead, student confusion)

**Student experience**:
- **Manual push**: Excellent (git pull, solutions appear)
- **GitHub Actions**: Excellent (when working)
- **Protected branch**: Good (git pull after unlock)
- **Separate repo**: Poor (navigate to different repo)

### The 2-Minute Post-Class Workflow

```bash
git checkout main
git merge solutions --no-ff -m "Release Lab X solutions"
git push origin main
# Post Canvas announcement
```

Done. No infrastructure. No debugging. No delays.

---

## Risk Analysis

### What Could Go Wrong?

| Approach | Risk | Mitigation |
|----------|------|-----------|
| **Manual push** | Forget to release | Calendar reminder |
| **Manual push** | Merge conflict | Resolve manually (rare, 5 min) |
| **GitHub Actions** | Workflow delays 15-30 min | Build in buffer time |
| **GitHub Actions** | Workflow fails silently | Add notifications, monitoring |
| **GitHub Actions** | Syntax error in YAML | Test thoroughly, maintain workflow |
| **Protected branch** | Forget to unlock | Calendar reminder |
| **Separate repo** | Wrong permissions set | Double-check access list |

### Failure Modes

**Manual push worst case**: Forget to release → Post solutions 1 hour late → Send apology email
- **Impact**: Low (students get solutions same day)
- **Recovery**: 5 minutes

**GitHub Actions worst case**: Workflow fails silently → Students email asking for solutions → Debug workflow mid-semester → Release manually anyway
- **Impact**: Medium (lost time debugging during busy semester)
- **Recovery**: 30-60 minutes

---

## Decision Tree

```
Do you have 50+ labs/semester?
├─ YES → Consider GitHub Actions (setup cost amortizes)
└─ NO (HPM 883 = 12 labs) ───┐
                              │
    Do you need exact timing (±5 min)?
    ├─ YES → Manual push (GitHub Actions can delay 15-30 min)
    └─ NO ───┐
              │
        Are you comfortable debugging workflows mid-semester?
        ├─ YES → GitHub Actions viable
        └─ NO → Manual push (zero debugging)

RESULT for HPM 883: Manual push
```

---

## Implementation Path

### This Week (5 minutes)
1. Create `solutions` branch in template repo
2. Add branch protection to `solutions`
3. Add calendar reminder after each class: "Release lab solutions"

### Each Week (2 minutes)
1. Before class: Commit solutions to `solutions` branch
2. After class: Merge `solutions` → `main`, push
3. Post Canvas announcement

### End of Semester
- Total time spent: ~29 minutes
- Reliability achieved: 100%
- Debugging time: 0 minutes
- Student satisfaction: High (integrated experience)

---

## Key Insight

**Premature optimization**: Spending 60-120 minutes setting up GitHub Actions to save 24 minutes of manual work across a semester is not a good trade-off, especially when:
1. Setup time exceeds the time saved
2. Adds debugging complexity mid-semester
3. Introduces reliability issues (scheduling delays)
4. Manual approach is already fast (2 min/lab)

**The best system is the one you'll actually use consistently.**

---

## Reassessment Triggers

Reconsider approach if:
- Scale to 50+ labs/semester (automation ROI improves)
- Multiple TAs need to coordinate releases (centralized automation helps)
- Instructor frequently unavailable after class (automation required)
- GitHub Classroom adds native solution release features (use built-in features)

Until then: Keep it simple.

---

## Final Recommendation

**For HPM 883 Spring 2026**:

✅ Use simple manual push (Option 5)
- Setup: 5 minutes this week
- Execution: 2 minutes per lab after class
- Total: 29 minutes entire semester
- Reliability: 100%
- Debugging: 0 minutes

**Implementation docs**:
- Analysis: `solution-release-approaches.md`
- How-to guide: `solution-release-implementation.md`
- This decision guide: `SOLUTION-RELEASE-DECISION.md`
