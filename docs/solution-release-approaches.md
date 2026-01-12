# Auto-Releasing Lab Solutions for HPM 883

**Context**: GitHub Classroom assignment repos with `analysis.qmd` (student work) and `analysis-sols.qmd` (solutions). Solutions should become visible AFTER each class session ends.

**Date**: 2026-01-11

---

## Executive Summary

**Recommended Approach: Simple Manual Push (Option 5)**

For HPM 883 with 12 weekly labs, the simplest reliable approach is:
- Store solutions in a protected `solutions` branch
- After class ends, manually merge to `main` (2-minute task)
- Students pull updated repos to see solutions

**Why this wins**: Zero infrastructure, 100% reliable, minimal ongoing effort (24 minutes total for semester), no complex debugging.

---

## Detailed Analysis

### Option 1: GitHub Actions Scheduled Workflow ⚠️

**How it works**: Workflow triggers at set time to make solutions visible (merge branch, create release, or push files).

**Pros**:
- Fully automated once configured
- Transparent (workflow file shows logic)
- No manual intervention after setup

**Cons**:
- **Setup complexity**: Medium-High (YAML workflow, cron syntax, timezone conversion)
- **Reliability issues**: GitHub Actions scheduled workflows can experience 15-30 minute delays during peak load
- **Maintenance**: Debugging failed workflows, handling edge cases
- **UTC timezone conversions**: 2 PM EST requires different cron expressions for EST vs EDT
- **Testing overhead**: Cannot easily test until scheduled time arrives

**Verdict**: Not recommended for time-sensitive releases with 12 assignments.

**Sources**:
- [GitHub Actions Scheduling Documentation](https://docs.github.com/actions/learn-github-actions/events-that-trigger-workflows)
- [Scheduled Actions Blog Post](https://jasonet.co/posts/scheduled-actions/)
- [Community Discussion on Schedule Delays](https://github.com/orgs/community/discussions/156282)

---

### Option 2: Protected Branch + Manual Unlock ⚠️

**How it works**: Solutions live in protected branch, instructor removes protection after class.

**Pros**:
- Solutions safely isolated before deadline
- Students can pull solutions after unlock
- Clear separation between student work and solutions

**Cons**:
- **Setup complexity**: Low-Medium (configure branch protection rules)
- **Ongoing manual steps**: Must remember to unlock each week (12 times/semester)
- **Risk of forgetting**: No automatic reminder to unlock
- **GitHub UI friction**: Navigate Settings → Branches → Edit protection → Save
- **Doesn't prevent pre-deadline viewing**: Students with repo access can see protected branch exists

**Verdict**: Manual unlocking every week is easy to forget and adds friction.

**Sources**:
- [GitHub Protected Branches Documentation](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [Managing Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule)

---

### Option 3: Separate Solutions Repo ⚠️

**How it works**: Maintain `HPM883-solutions` repo, grant students read access after class.

**Pros**:
- Clean separation between student work and solutions
- Simple access control (one repo to manage)

**Cons**:
- **Setup complexity**: Medium (create repo, manage access list)
- **Ongoing manual steps**: Update repo permissions 12 times/semester
- **Poor UX**: Students must visit separate repo instead of seeing solutions in their workspace
- **Access management overhead**: Manually add all students, handle drops/adds
- **No integration**: Solutions not alongside student code for comparison

**Verdict**: Adds administrative burden without clear benefit over simpler approaches.

---

### Option 4: GitHub Classroom Feedback Features ⚠️

**How it works**: Use GitHub Classroom's built-in feedback mechanisms.

**Research findings**: GitHub Classroom currently **lacks native solution release features**. Key limitations:

- **No automatic solution release**: No built-in way to release solutions at specific times
- **Feedback pull requests**: Exist, but don't support scheduled/conditional solution release
- **Deadline cutoffs**: Prevent student writes, but don't control solution visibility
- **Community requests**: Multiple open feature requests for solution release (Issue #527, #502)

**Pros**:
- Would be ideal if it existed

**Cons**:
- **Not available**: Feature does not exist as of January 2026
- **Workarounds required**: All other approaches on this list

**Verdict**: Not a viable option currently. Monitor for future platform updates.

**Sources**:
- [GitHub Classroom Issue #527 - Solution Release Feature Request](https://github.com/education/classroom/issues/527)
- [GitHub Classroom Issue #502 - Related Solution Distribution Request](https://github.com/education/classroom/issues/502)
- [GitHub Education Community Discussion on Solution Release](https://github.com/orgs/community/discussions/80909)

---

### Option 5: Simple Manual Push ✅ RECOMMENDED

**How it works**:
1. Store solutions in protected `solutions` branch (prevents accidental student access)
2. After class ends, instructor runs:
   ```bash
   git checkout main
   git merge solutions --no-ff -m "Release Lab X solutions"
   git push origin main
   ```
3. Students pull updated repos, see solutions appear in `analysis-sols.qmd`

**Pros**:
- **Minimal setup**: Create `solutions` branch, add branch protection (5 minutes one-time)
- **100% reliable**: No infrastructure dependencies, scheduling delays, or API rate limits
- **Fast execution**: 2 minutes per lab after class
- **Easy to test**: Can test locally before pushing
- **Clear audit trail**: Git history shows exactly when solutions were released
- **Flexible timing**: Can release early/late based on actual class progress
- **Zero debugging**: No workflows to debug, no YAML syntax errors

**Cons**:
- **Manual effort**: Must remember to run commands after each class (12 times/semester = ~24 minutes total)
- **Requires git access**: Instructor must have repo access and basic git knowledge

**Workflow setup** (one-time, 5 minutes):
```bash
# Create solutions branch from main
git checkout -b solutions

# Add solutions to analysis-sols.qmd
# Commit solutions
git add analysis-sols.qmd
git commit -m "Add Lab 1 solutions"
git push origin solutions

# Protect solutions branch on GitHub:
# Settings → Branches → Add branch protection rule
# Branch name: solutions
# Check: "Require a pull request before merging"
# Save
```

**Weekly workflow** (2 minutes):
```bash
# After class ends
cd ~/path/to/repo
git checkout main
git merge solutions --no-ff -m "Release Lab X solutions"
git push origin main

# Notify students via Canvas/Slack:
# "Lab X solutions now available - git pull to update your repo"
```

**Verdict**: Best balance of simplicity, reliability, and effort for 12 labs/semester.

---

## Comparison Matrix

| Criterion | Manual Push | Protected Branch | Separate Repo | GitHub Actions | Classroom Features |
|-----------|-------------|------------------|---------------|----------------|-------------------|
| Setup time | 5 min | 10 min | 20 min | 60+ min | N/A |
| Ongoing effort/lab | 2 min | 3 min | 5 min | 0 min (when working) | N/A |
| Total semester effort | 24 min | 36 min | 60 min | ?? (debugging) | N/A |
| Reliability | 100% | 95% | 90% | 70-85% | N/A |
| Debugging complexity | None | Low | Medium | High | N/A |
| Flexibility | High | Medium | Low | Low | N/A |
| Student UX | Excellent | Good | Poor | Excellent | N/A |

---

## Implementation Recommendation

**For HPM 883 Spring 2026**: Use **Option 5 (Simple Manual Push)**.

**Rationale**:
- 12 labs × 2 minutes = 24 minutes total manual effort for entire semester
- Zero infrastructure complexity
- No risk of scheduled workflow delays causing solutions to release late
- Can adapt timing based on actual class pace
- Students get excellent UX (solutions appear in their existing repo)
- Clear git history for audit trail

**Setup steps**:
1. Create `solutions` branch in assignment template repo
2. Add branch protection to `solutions` (prevent accidental deletion)
3. Commit solutions to `solutions` branch as you create them
4. After each class, merge `solutions` → `main` and push

**Optional enhancements**:
- Set calendar reminder 5 minutes after class ends
- Create shell alias: `alias release-lab='git checkout main && git merge solutions --no-ff && git push origin main'`
- Add notification template to Canvas for announcing solution availability

---

## When to Reconsider

Consider **GitHub Actions** (Option 1) IF:
- Course scales to 50+ labs/semester (manual effort exceeds setup cost)
- Multiple instructors need to coordinate releases
- Exact timing precision (±5 minutes) is not critical
- You have time to debug workflow issues mid-semester

Consider **Separate Repo** (Option 3) IF:
- Solutions contain sensitive information not suitable for student repos
- Need fine-grained access control (some students get solutions, others don't)

Monitor **GitHub Classroom** (Option 4) for future updates:
- Check GitHub Classroom release notes each semester
- Feature requests #527, #502 show community demand
- May become viable in future academic years

---

## Additional Resources

- [GitHub Classroom Documentation](https://docs.github.com/en/education/manage-coursework-with-github-classroom)
- [GitHub Actions for Education Guide](https://docs.github.com/en/actions/tutorials/manage-your-work/schedule-issue-creation)
- [Protected Branches Guide](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches)

---

## Appendix: Alternative Approaches Not Recommended

### Git Tags for Release
- **Idea**: Create git tag after class, students checkout tag
- **Why not**: Students must know to fetch tags, adds friction vs. simple pull

### Private Submodules
- **Idea**: Solutions as private submodule, update pointer after class
- **Why not**: Submodule complexity far exceeds benefit for 12 labs

### Draft Releases on Schedule
- **Idea**: Pre-create draft releases, unpublish on schedule via Actions
- **Why not**: Still requires GitHub Actions scheduling (inherits same reliability issues)

### LMS Integration
- **Idea**: Post solutions to Canvas after deadline
- **Why not**: Fragments student experience (code in GitHub, solutions in Canvas)
