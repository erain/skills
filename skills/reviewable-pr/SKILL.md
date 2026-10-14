---
name: reviewable-pr
description: Take a scoped code change through implementation, automated tests, end-to-end validation, and a clear upstream draft PR. Use when implementing a fix or feature for human review; confirm whether an optional terminal demo is wanted before recording one.
---

# Reviewable PR

Deliver a change a human can understand and verify: explain its behavior, prove
that behavior with tests, and prepare a draft PR with concise evidence.

Example invocations:

- “Use $reviewable-pr to fix this issue and open an upstream draft PR.”
- “Follow skills/reviewable-pr/SKILL.md for this change. Include a live e2e test.”

## Understand the change

Read the issue, current implementation, and repository contribution rules.
Check the working tree and preserve unrelated work. Verify that the issue's
suggested approach still fits the current architecture.

Identify the observable behavior before and after the change. For a lifecycle
change, name the states, what triggers transitions, and what happens on failure,
restart, and cancellation. Implement the narrowest change that achieves it.

For work requiring prior issue discussion, record the proposed behavior and
validation on the issue when authorized. Keep ordinary implementation decisions
moving; ask only about choices that materially affect scope or behavior.

## Confirm whether a recording is useful

A recording is optional. It is most useful when timing, several interacting
services, or a sequence of state changes is hard to convey in a test result.
A small local fix usually needs only tests and a written explanation.

Unless the user has already explicitly requested or declined recording **for
this change**, ask early:

> Would you like a short terminal demo/GIF for this change? I recommend
> [recording / skipping it] because [one concrete reason].

Continue implementation and tests while waiting. Do not record, install
recording tools, or upload a recording without an affirmative answer. No answer
means continue without a recording; a later answer can add one. This choice
must not block the code, tests, or draft PR.

If requested, read [references/terminal-demo.md](references/terminal-demo.md)
after designing the test. Use the test itself as the demo.

## Build and test the behavior

Follow the project's formatter, build, test, and verify/lint commands. Reuse
existing test fixtures and standard testing conventions.

- Add regression tests for the reported failure and the intended result.
  For a bug fix, demonstrate failure against the old implementation when
  practical, then success with the fix. An expected baseline failure is evidence,
  not a failure of the final change.
- Test consequential boundaries: transient versus permanent failures,
  retry/backoff behavior, restart recovery, and cancellation when they apply.
  Choose cases from the design rather than adding a generic checklist to every fix.
- Run the relevant tests, project verification, and build. Report failures and
  omitted checks accurately. Repeat checks when a change or failure warrants it.
- Re-read the complete diff, including new files. Check that the test proves an
  observable outcome rather than copying the implementation.

## Exercise the complete path when needed

Use end-to-end tests when the risk crosses components or depends on real
infrastructure. A lifecycle bug may pass unit tests while still failing through
the CLI, API, persistence layer, or runtime.

Make the test runnable with one documented command and explicit prerequisites.
Use isolated fixtures and bounded waits; clean up only resources the test owns,
including after failure. Exercise public entry points and assert outcomes.
For an execution feature, actually run a command or inspect its result; a
`Running` status alone does not prove it works.

For example, a capacity-recovery test can fill an isolated worker pool, submit
more work, check that it waits, free or add capacity, and verify automatic
execution. Check restart and user cancellation if the design promises them.
Do not copy service-specific assumptions into unrelated projects.

Missing prerequisites must be visible. An explicitly invoked live test should
fail with a useful setup error instead of silently skipping. If the project has
optional local tests, follow its convention but require prerequisites in CI.
Clearly distinguish CI tests from manually run live tests.

Keep the test script, fixtures, and short setup instructions in the same PR when
they form a reusable test. Disclose fixture substitutions or coverage limits
that affect what a passing result proves. If the demo depends on a one-off local
setup, put its script and setup in a gist or another permitted artifact host
rather than introducing a second PR just for review artifacts.

## Explain and prepare the draft PR

Write for someone who has not read the conversation. Lead with the problem and
new behavior, then explain the design in a few sentences. For a lifecycle change,
show the new flow in a short numbered list or a small diagram if that is clearer.
Describe what was added and why it is needed; avoid a file-by-file change log.

Explain live validation in plain terms:

- **Setup:** the real components and the relevant capacity or configuration.
- **Actions and expected outcomes:** what the test does and what should happen.
- **Result:** what passed, with any material limitation or check not run.

Include the GIF if requested and useful. The image or video is sufficient;
extra links to original recordings, conversion details, or artifact history do
not belong in the PR description unless they help reproduce or assess the test.
Keep detailed commands in the test README. Include the issue reference and an
honest AI-assistance sentence when required; never claim human review happened
on the user's behalf or add AI attribution to commits.

Prefer one focused PR containing the implementation and its tests. Do not split
off tests merely to create another PR. Follow applicable repository sizing rules
and the user's chosen scope; split independent changes when warranted.

When authorized to prepare the PR, commit and push a feature branch and open the
PR against the upstream repository **in draft mode**. Use a fork if needed.
Do not mark it ready for review or merge it without the user's instruction.
Do not infer publication permission from a request for read-only advice.

Check the actual PR: draft state, target branch, final diff, rendered description,
and accessible demo. Inspect CI results and fix failures introduced by the change;
report checks still running without claiming they passed. Distinguish a pushed
branch, an open draft, and a merged change. Finish with the PR link and the few
validation facts the user needs.
