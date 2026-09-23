# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

Live-mode run of the `issue-select` skill on issue #68 ("Keyword search raises
`ZeroDivisionError` when the index is empty"). Summary, one line per check:

- maintainer-alive: pass — Aburke225 committed 2026-09-16, 6 days before today
- repo-in-use: pass — archived: no; last push 2026-09-16 (<365d); repo cuts no releases
- scope-fits-newcomer: pass — one bounded fix (guard empty corpus in `index()`); existing xfail test H-01 to unmark; est 2-4h
- not-claimed: pass — assignees: none; linked PRs: none; no comments
- ai-policy-allows: pass — CONTRIBUTING.md has no AI statement (silence passes); repo commits are co-authored by claude
- good-first-signal (preferred): pass — labels `good first issue`, `tier-1`; built-in acceptance test
- adoption-scale (preferred): fail — 4 stars (<1000); ranking only, does not change verdict

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "Aburke225 committed 2026-09-16, 6 days before today"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "archived: no; last push 2026-09-16 (<365d); repo cuts no releases"},
    {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "one bounded fix: guard empty corpus in index(); existing xfail test H-01 to unmark; est 2-4h"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; no comments"},
    {"name": "ai-policy-allows", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI statement (silence passes); repo commits co-authored by claude"},
    {"name": "good-first-signal", "grade": "pass", "evidence": "labels: good first issue, tier-1; built-in acceptance test"},
    {"name": "adoption-scale", "grade": "fail", "evidence": "4 stars (<1000); ranking only, does not change verdict"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

I ran the full 20-issue eval set twice on Sonnet.

1. First run (no `--save-run`): agreement **19/20 — PASS** (bar 18/20). One miss: `issue-19`.
2. Committed run (`--save-run eval-run.txt`): agreement **18/20 — PASS**, categories
   `claimed 4/4  clear-accept 6/8  dead-repo 3/3  policy 1/1  scope 4/4` (category floor
   met). Two misses: `issue-01` and `issue-19`, both gold `accept`, both rejected on
   `scope-fits-newcomer`.

The last score (18/20) matches the agreement line in the committed `eval-run.txt`.

**Issue analysis**

`issue-19` — my rubric's decision: **reject**; gold label: **accept**.

The issue ("Selecting large subgraphs in proof mode freezes the UI") lists "two potential
causes which should be fixed" plus several "additional suggestions" (multi-processing,
threading). My `scope-fits-newcomer` check read that enumerated list as multiple tasks and
graded it like an umbrella issue, so it failed and the verdict was reject. But it is
actually one bounded performance bug, opened by a COLLABORATOR and labeled high-priority;
the extra items are contributing causes and optional directions, not separate
deliverables. The gold label treats it as an accept, and the evidence guide's own note
applies — "short is not the same as unscoped... grade the size of the work being asked
for, not the polish." So my check is slightly too strict on issues that enumerate causes.

**Check rationale**

Quoting the pass condition of `scope-fits-newcomer`, as currently written in the
`rubric.md` uploaded to `tools/issue-select/`:

> The issue asks for one bounded change (a specific bug fix, a small feature, or a doc
> fix). Fail if it is an umbrella/tracking issue, a pure usage/support question, has an
> unresolved design debate no maintainer has settled, carries a maintainer statement that
> the fix touches core internals, or shows several abandoned prior attempts. `unclear` if
> scope cannot be judged from the text

I wrote it as a list of concrete disqualifiers rather than an adjective like "small
enough," so a grader applies the same fail conditions I do. Naming the exact killers
(umbrella issue, support question, unsettled design, core-internals statement, repeated
abandoned attempts) is what lets the check separate a genuinely oversized issue from a
terse-but-bounded one, and `unclear -> fail` keeps an unverifiable scope from slipping
through as an accept.

**Trade-offs**

What this check gives up: a case I accept it will miss. In my committed run it rejected
`issue-01` and `issue-19` — both real accepts — because each enumerates several causes and
the check reads that as an umbrella. I chose not to loosen it, and nothing changed
elsewhere as a result: my four `scope` rejects stayed correct (`scope 4/4` in the
committed run's category line). Loosening the "umbrella/tracking issue" clause enough to
rescue those two accepts would risk flipping a genuinely out-of-scope reject into a wrong
accept, so I accept two false rejects on the accept side to keep the reject side clean.

---

## Selection rationale

**Selection rationale**

1. **Fit and time.** It is a Python bug, a language I already use, so I can spend my time
   on the fix rather than on learning a new stack, and it is none of the frontend/CSS work
   I wanted to avoid. It is tightly bounded (guard one empty-corpus case in `index()`,
   estimated 2-4 hours), which fits the time I have this unit.
2. **What the verdict got right, and what I weighed beyond it.** The rubric correctly
   confirmed the issue is unclaimed, on a live and in-use repo, in scope, and under no AI
   ban. What I weighed that the rubric does not score: this issue ships an existing
   `@pytest.mark.xfail` test (manifest id H-01) that I remove when the fix lands, so
   "done" is unambiguous. That built-in proof of correctness is why I ranked it above #62
   and #73, even though all three were accepted.
3. **Anticipated difficulty claiming it.** Low. This is a Path Review classroom repo, and
   the scope house rule says classmates' claim comments do not block an issue, so several
   students may be on #68 and that is fine — credit attaches to the PR I open, not to
   whether the issue is free. The main work is matching the rank-bm25 library's empty-input
   behavior and removing the xfail marker cleanly.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
