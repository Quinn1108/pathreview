## Week 7 — Issue selection

**Issue link:** (https://github.com/ascherj/pathreview/issues/157)

**Issue title:** Relevance scorer “partial overlap” test fixture actually has full query overlap

**Tier:** [X] Tier 1  [ ] Tier 2  [ ] Tier 3


**Problem summary:**

The unit test test_query_with_partial_overlap in tests/unit/test_relevance_scorer.py is checking that a query with only partial keyword overlap against a chunk scores below 0.9, but the fixture chunk actually contains all four query terms ("Python Django web framework"), giving full coverage. Since the relevance scorer correctly returns 1.0 when every query term is present, the test fails not because the scorer is broken, but because the fixture doesn't represent a genuinely partial-overlap scenario. The fix is to update the chunk fixture so it contains only some of the query terms (e.g., drop "Django" and "framework"), making the partial-overlap assertion meaningful. This affects only the test fixture in the relevance scorer test suite, not the scorer implementation itself. A successful fix will make the test accurately validate partial-match scoring behavior and pass without masking or weakening the underlying scoring logic.

**Branch name:** fix/157-test-coverage-error

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger