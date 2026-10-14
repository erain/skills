# Optional terminal demo

Read this only after the user requests a recording for the current change.

## Prepare a reproducible run

Use the end-to-end test as the recording script, with real assertions and a
nonzero exit on failure. Print the action being exercised, the resulting state,
and a short success message. Prefer meaningful transitions over verbose logs.
A reviewer should be able to understand the setup and outcome without watching
an installation or build. Prepare dependencies before starting the recording.

For a regression, consider showing the same scenario fail on the old code and
pass on the fix. Label the two runs clearly. Confirm that the baseline failed
for the reported bug, not a missing prerequisite. Do not hide an unexpected
failure or manufacture successful output. Check the command's exit status and
final assertions independently of the recorder's exit status.

Freeze the script and binaries for a take. **Do not edit a shell script while it
is executing:** a shell may read later lines from the changed file. If an edit
is needed, stop the run, clean up its owned resources, and start a new take.
Use a separate output file per take to avoid concurrent writers corrupting it.

Give every external wait a timeout. When a script owns a background service or
port-forward, launch the actual executable, keep its PID, and terminate and wait
for it in cleanup. Backgrounding a shell wrapper can leave its child running.
Keep fixtures isolated; capacity tests must not consume unrelated workloads.

## Capture and convert

`asciinema` captures terminal text and timing; `agg` converts the cast to a GIF.
Check available tools and their `--help` first, since installed versions differ.
Install missing tools only within the environment's authorization and conventions.
The commands below are a starting point, not a mandatory dependency of the skill:

```bash
# Replace this path with the project's tested demo entry point.
asciinema rec --command './test/e2e/demo.sh' demo.cast
agg --idle-time-limit 2 demo.cast demo.gif
```

Use a new filename for each run. Choose terminal dimensions and font size so
commands, important state changes, and the final result remain readable at the
size GitHub will display. Keep the demo short by removing setup noise and
compressing idle time in the GIF; do not shorten the test's real waits or change
its assertions for presentation. Describe idle-time compression if timing itself
is part of the evidence. Keep the cast locally for debugging and regeneration.

Inspect the generated GIF at the beginning, a key transition, and the end.
Verify that the final output belongs to a completed passing run. Recordings can
expose shell history, credentials, environment values, or unrelated work: run
only the intended demo and inspect the output before sharing it.

## Share the useful evidence

Respect the repository's artifact policy. If binary files are prohibited, host
the GIF in a permitted location and embed its URL in the PR. A gist is suitable
for text scripts, manifests, logs, or the cast; do not assume it supports a binary
GIF attachment. Use GitHub attachments or another authorized host for the GIF.
Do not change the repository's binary policy just to store a demo.

The request to record does not by itself authorize uploading to a new public
service. Use a destination already authorized for sharing the PR/demo, or confirm
the destination before an additional public upload. No asciinema.org account or
upload is required. Check that the published asset loads and matches the local
file, rather than relying only on an HTTP status.

Embed the GIF or video alongside a concise description of the setup, tested
actions, and expected outcomes. Avoid creating a separate artifact PR or adding
raw recordings to the source diff when a link provides enough evidence.
