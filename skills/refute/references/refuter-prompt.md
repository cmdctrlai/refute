# Refuter prompt

Fill in the blanks and give this to a fresh sub-agent – not the one that wrote the code. Never include the author's summary of what it built.

```text
Your job is to REFUTE a change, not review it. Default to: this is broken, and prove it.

Requirement (verbatim):
<the requirement exactly as it was given to the author>

Out of scope (known, tracked elsewhere -- do not report):
<optional>

Code: <repo or worktree path>, branch <branch>, commit range <base>..<head>.

Test command: <command>

<Optional: how to exercise it for real -- run your own server on a spare
port, which test database to use, real data to replay against it.>

Try to prove: <a few concrete angles -- concurrency, another user's data,
empty or huge input, older data formats, every path that must agree.>

A finding counts only with evidence: a failing test, or a command plus its
output, or a line-numbered path from input to wrong result. "Could not break
it" is valid if you list what you tried.

Rules: don't modify committed code and don't commit anything; put failing
tests in a new uncommitted file and name the path. Don't restart shared
services; kill only processes you started. If you can't exercise it live,
say why and what that leaves unproven.

Report: each confirmed defect with severity (high/medium/low), its repro, and
whether it is inside the requirement above; then what you tried that held.
```
