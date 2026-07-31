## Week 7 — Issue selection

**Issue link:** (https://github.com/ascherj/pathreview/issues/157)

**Issue title:** Relevance scorer “partial overlap” test fixture actually has full query overlap

**Tier:** [X] Tier 1  [ ] Tier 2  [ ] Tier 3

**Files to touch**: tests/unit/test_relevance_scorer.py only — the fix is confined to the fixture data in the test file itself. No changes needed to the scorer source, since the scorer's behavior (returning 1.0 for full keyword coverage) is correct.

**Estimated time**: ~30 minutes. This is a small, well-understood fix: adjust the fixture chunk so it contains a genuine subset of the query terms rather than all four, re-run the test, confirm it passes.
Prerequisites: None. The issue is isolated to test fixture design — no dependency on understanding the scorer's internals, no upstream/downstream code to account for, and no environment setup beyond what's already in place.

**Verdict**: Right-sized for a Tier 1 pick — self-contained, low-risk, and doesn't require touching production logic.

**Problem summary:**

The unit test test_query_with_partial_overlap in tests/unit/test_relevance_scorer.py is checking that a query with only partial keyword overlap against a chunk scores below 0.9, but the fixture chunk actually contains all four query terms ("Python Django web framework"), giving full coverage. Since the relevance scorer correctly returns 1.0 when every query term is present, the test fails not because the scorer is broken, but because the fixture doesn't represent a genuinely partial-overlap scenario. The fix is to update the chunk fixture so it contains only some of the query terms (e.g., drop "Django" and "framework"), making the partial-overlap assertion meaningful. This affects only the test fixture in the relevance scorer test suite, not the scorer implementation itself. A successful fix will make the test accurately validate partial-match scoring behavior and pass without masking or weakening the underlying scoring logic.

**Branch name:** fix/157-test-coverage-error

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger

## 20260721 — Reproduced: test_query_with_partial_overlap failure

**Command:** `pytest tests/unit/test_relevance_scorer.py -q`
**Observed failure:** `assert 1.0 < 0.9`

**Root cause:** The test fixture's chunk contains all four terms from the
query "Python Django web framework" — full keyword coverage, not partial
overlap as the test name implies. The relevance scorer correctly returns
1.0 for full coverage, so the assertion `score < 0.9` fails against
correct behavior, not a scorer bug.

**Fix (not yet applied):** Rewrite the fixture chunk so it contains only
a subset of the query terms, producing genuine partial overlap.

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [link to commit documenting the reproduced issue]

**Reproduction summary:**
I ran pytest tests/unit/test_relevance_scorer.py -q and observed the assertion assert 1.0 < 0.9 fail in test_query_with_partial_overlap, confirming the scorer returns a full score of 1.0 because the fixture chunk contains all four terms from the query ("Python Django web framework"), rather than a partial subset as the test name and assertion intend.

**PLAN.md link:** (https://github.com/Quinn1108/pathreview/blob/fix/157-test-coverage-error/plan.md)

**Blockers or open questions:**

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
[What have you implemented so far? Which sub-tasks from PLAN.md are done?]

**Next steps:**
[What are you working on for the rest of the week?]

**Blockers:**
[Anything slowing you down? Or leave blank.]

---

### Check-in 2 (end of week)

**PR link:** [link to your submitted pull request]

**Branch:** [the branch name you worked on, e.g. `fix/123-short-description`]

**What you built:**
[1–3 sentences summarizing what your fix does and how it works]

**Tests added or updated:**
[Which test files did you touch? What do they cover?]

**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes

**Draft PR feedback received from:** [name or Slack handle, or "none"]