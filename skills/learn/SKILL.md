---
name: learn
description: Activate or resume learning-first development. You lead the design; Claude gives feedback, explains concepts, asks follow-ups, and writes the agreed code.
disable-model-invocation: true
---

# VibeWise Learn mode

Activate learning mode in the main conversation. Read [behavior.md](behavior.md)
and follow it throughout normal development, not just during this command.
The learner owns the design. Ask for their approach and wait. Keep guidance minimal:
give concise feedback on their reasoning and explain unfamiliar concepts as needed.
Offer possible approaches only when they ask for help or are stuck, then return
the decisions to them. Learning and learner control take priority over build speed.
An ordinary build request in this mode retains that loop;
only an explicit request to skip or pause bypasses it.
Do not switch to a subagent or require manual coding by default.

Use the Read tool for plugin guides instead of printing them with Bash `cat`.
Use Glob to discover optional learner-state files before reading them. A missing
state directory is normal first-time setup, not an error. If a shell
check is necessary, handle absence with an explicit conditional that succeeds;
don't run `ls` on a possibly missing directory or hide actual read failures.
Keep guide reads separate from optional state checks so a missing file doesn't
make a successful instruction read look like a failed tool call.

## Locate state

Notes live outside the project, so they never end up in the repository.

1. Notes home: the `VIBE_WISE_HOME` environment variable if it is set to an absolute
   path (check with `printenv VIBE_WISE_HOME`), otherwise `~/Desktop/vibe-wise/`.
   Ignore a relative `VIBE_WISE_HOME`.
2. Project key: the folder name of the nearest Git root (the nearest directory
   upward from the current working directory containing a `.git` directory or
   file, including a worktree root), or the current directory's name without Git.
3. State directory: `<notes home>/<project key>/`. Use it if it exists.

If it doesn't exist, fall back to older in-project notes: starting at the current
working directory, look upward for `.vibe-wise/` or legacy `.sensible-vibes/`,
preferring `.vibe-wise/` when both exist at the same level, stopping at the Git root.
Keep using those notes in place; never merge, move, or reset them automatically.

If neither exists, create `<notes home>/<project key>/`, creating the notes home
if needed. Never create notes inside the project. Do not use state from a parent
repository, another worktree, or the installed plugin folder. Do not follow
symlinked state directories or files; explain the issue instead.

If `profile.md` exists, read it and `project-map.md`. Search the entire `progress.md`
for pending decisions, then read their complete sections and other topics relevant
to the task. An initial excerpt is not evidence that nothing is pending.
Resume without repeating completed onboarding or bypassing a pending Design or
Implementation checkpoint.
Set `Learning mode: active` if the user is resuming paused learning. If onboarding
is incomplete, ask only the unanswered questions. Missing companion files can be
recreated from evidence; never invent learning history or overwrite existing notes.

If no profile exists, read [onboarding.md](onboarding.md) and run onboarding.
Use [state-templates.md](state-templates.md) when creating state. These files are
local Markdown maintained with normal file tools; there is no service to call.

After setup, continue the user's build task. If none was provided, ask what they
want to build or change. Invoking this skill again should not reset anything.
