# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

All recency thresholds are measured against the bundle's capture date in
eval mode, and against today in live mode (see
`references/evidence-guide.md`, "Reading the repo-facts block honestly").

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `maintainer-alive` | Repo facts: "last 5 default-branch commits" (dates + authors) and "maintainer first-response sample"; in eval also the Comments' `author_association`, in live the Owner/Member/Collaborator badges in the thread (evidence-guide Family 1) | At least one of: (a) a non-bot human commit (author not ending in `[bot]`) on the default branch within 180 days; (b) the response sample shows at least one issue answered by an Owner/Member/Collaborator within 60 days. Fail if neither holds | required |
| `repo-in-use` | Repo facts: `archived:` flag, "last push to any branch", "latest release" (evidence-guide Family 2) | `archived: no` AND last push within 365 days AND a latest release exists dated within 730 days (or a push within 365 days if the project cuts no releases). Fail if archived, or if the newest of push/release is older than 365 days | required |
| `scope-fits-newcomer` | The issue title/body, its labels, and the full comment thread (evidence-guide Family 3) | The issue asks for one bounded change (a specific bug fix, a small feature, or a doc fix). Fail if it is an umbrella/tracking issue, a pure usage/support question, has an unresolved design debate no maintainer has settled, carries a maintainer statement that the fix touches core internals, or shows several abandoned prior attempts. `unclear` if scope cannot be judged from the text | required |
| `not-claimed` | Repo facts: "this issue: assignees" and "linked PRs" with state; plus claim language ("I'll take this", "working on this") in the comment thread (evidence-guide Family 4). In live Path Review mode apply the scope.md house rule: classmates' claim comments do not count | No assignee, AND no open linked PR, AND no active unresolved claim comment. A closed/unmerged linked PR is an abandoned attempt, not a live claim (but weigh it under `scope-fits-newcomer`). Fail on any live claim | required |
| `ai-policy-allows` | Repo facts: "contribution policy" line (CONTRIBUTING.md, any AI policy file, PR/issue templates) (evidence-guide "fifth surface") | No outright ban on AI-assisted contributions. Conditions (disclose AI use, understand/test/human-review every change, "no fully AI-generated but assistive allowed") PASS; silence PASSES. Fail only on a stated ban that reaches assistive/AI-assisted work | required |
| `good-first-signal` | The issue's labels and body: a `good first issue`/`help wanted` label, an acceptance-criteria checklist, or the issue opened by a maintainer | Passes when any such friendliness signal is present; used only to rank accepted issues higher | preferred |
| `adoption-scale` | Repo facts: star count on the repo line (evidence-guide Family 2, "adoption scale") | Passes when stars ≥ 1000; a merged PR on a widely-used repo is worth more. Ranking only | preferred |

## Verdict rule

Accept if and only if every `required` check grades `pass`. Any `required`
check that grades `fail` rejects the issue. `unclear` on a `required` check
counts as `fail`: a first issue whose safety I cannot verify from the
evidence is not one to take.

`preferred` checks never change the verdict. They are reported and, among
accepted issues, used together with the scope.md fit profile to rank which
accepted candidate to take first.
