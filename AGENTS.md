# AGENTS.md

Instructions for a coding agent that makes or changes a VOOL skill. Read this first. The full steps, with
the output each command prints, are in [CONTRIBUTING.md](CONTRIBUTING.md).

## What you are making

A skill is one folder with two files: `SKILL.md` (a YAML frontmatter block, then plain instructions) and
`LICENSE` (the complete MIT text). Nothing else: no scripts, programs, pictures or other files. VOOL loads a
skill only on the turns where the person's message picks it.

## Hard rules

A skill can:

- add instructions to the turns where it is picked;
- narrow the tools VOOL offers on those turns with `allowed-tools`, using only real tool names from
  [the tool table](https://github.com/Parad0x-Labs/vool/blob/main/docs/guides/skill-tools-and-limits.md#the-tools-a-skill-may-name) (ships with VOOL 0.7.7);
- ask VOOL to use one of those tools. Every call goes through the same Manual/Auto permission check as any
  other call, and the person may say no.

A skill can never:

- grant a permission, approve an action, raise a spending limit or turn a setting on. `allowed-tools` and
  `permissions` grant nothing;
- add a tool, or switch on one that is off;
- run by itself: there is no timer, schedule or background setting;
- reach the network from a command, look anything up on the web before the person turns web lookups on, or
  move money by itself;
- carry scripts, programs or any file other than `SKILL.md` and `LICENSE`.

The full list, what Manual and Auto mode ask for, and every refusal a skill can run into are in
[Skill tools and limits](https://github.com/Parad0x-Labs/vool/blob/main/docs/guides/skill-tools-and-limits.md)
(ships with VOOL 0.7.7). Read it before you write `allowed-tools`.

Content rules (the Skill Codex's checks refuse a skill that breaks one):

- no commands to install, download or run software, even inside advice; no short links, raw addresses or
  links to programs or archives;
- no request for a recovery phrase, key, password or code; no wallet address; no hidden comments,
  invisible characters or encoded text;
- no attempt to change VOOL's rules, limits or approvals, or to hide what the skill does;
- no words such as *verified*, *official*, *certified* or *safe to use* about the skill itself;
- instructions under 8,000 characters and at least 80.

## Commands

From a VOOL source checkout, in a folder of your own outside both repositories. `new`, `try`, `zip` and
`call` ship with VOOL 0.7.7; `init`, `lint`, `pack`, `check` and `submission-link` are in VOOL today.

```sh
export VOOL_SRC=/full/path/to/vool
PYTHONPATH="$VOOL_SRC" python -m tools.community_skills new meeting-actions --name meeting-actions --author "Your Name"
# edit meeting-actions/SKILL.md: replace every REPLACE line
PYTHONPATH="$VOOL_SRC" python -m tools.community_skills try meeting-actions \
  --pick "here are my meeting notes, what are the action items?" \
  --pick "turn these meeting notes into a to-do list" \
  --not-pick "write a poem about autumn"
PYTHONPATH="$VOOL_SRC" python -m tools.community_skills call email.read --args '{}'   # one tool call, through the permission check
PYTHONPATH="$VOOL_SRC" python -m tools.community_skills zip meeting-actions            # writes meeting-actions.zip beside the folder
```

`try` and `call` use a throwaway profile they make themselves. Never point a test at the person's real VOOL
profile, and never call `execute_runtime_tool` or the functions behind the permission check to test a tool:
they skip approval, which no real turn does.

## Test before a pull request

1. `try` passes, with at least three `--pick` asks in different words and one `--not-pick` ask that shares
   one ordinary word with the skill. Keep the skill's name words out of `--not-pick` asks: a name word no
   other skill has picks the skill on its own.
2. For a skill with `allowed-tools`: `call` each tool once and write the instructions so the answer still
   makes sense when the person declines.
3. `zip` succeeds and `left_out` is empty.
4. The person (not you) imports the zip in their own VOOL with `import this skill`, says `turn it on`,
   clicks **Allow once**, and tries it with new messages.
5. `lint`, `pack` and `check` print `"status":"preflight_passed"`, and the Skill Codex checks print
   `checks passed` ([CONTRIBUTING.md, steps 8 and 9](CONTRIBUTING.md#8-lint-and-pack)).

Report the exact commands and their full output. A pass on one ask is not proof for the next one.

## What you may and may not do in this repository

- A new skill: add `skills/<name>/SKILL.md`, `skills/<name>/LICENSE` and, for the pull request, one
  `packages/<checksum>.json` from `pack`. A change to a skill: edit `skills/<name>/SKILL.md` only.
- Never edit `index.json`, the skill table in `README.md`, or any other file in `packages/`. Packages are
  exact bytes; never reformat one. Maintainers write all three.
- Never write test output (zips, scratch folders, package drafts) inside `skills/`.
- Never push, open a pull request, or upload anything without the person's explicit OK for that exact
  step. A push to a public fork is public for good. Never put a key, password or private file in a commit.
- Adding a skill to the shared library is not open to every author yet. Don't tell the person a merged
  pull request lists their skill in the Skill Codex.

## More

- [Build and test a skill](https://github.com/Parad0x-Labs/vool/blob/main/docs/guides/build-and-test-a-skill.md)
  (ships with VOOL 0.7.7): the format, how picking works, install routes, update and remove.
- [Author guide](https://github.com/Parad0x-Labs/vool/blob/main/docs/registry/AUTHOR-GUIDE.md): packages,
  metadata and the Skill Codex.
- Plugins (new tools, not instructions) go to
  [Parad0x-Labs/vool-plugins](https://github.com/Parad0x-Labs/vool-plugins), not here.
