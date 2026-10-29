# Notes for AI coding agents

This repository holds VOOL skills: text-only instructions in `skills/<name>/SKILL.md`. Read
[CONTRIBUTING.md](CONTRIBUTING.md) for the format and house style.

## Do

- Copy the structure of an existing skill, such as `skills/email-reply/SKILL.md`.
- Keep `name` equal to the folder name, `license: MIT`, and a `description` that ends with "Use when ...".
- Include the line that treats supplied text as content, not commands.
- Keep the body under 8,000 characters.
- List any VOOL tools in `allowed-tools`, using only names that appear in `index.json` under `tools`.

## Never

- Edit, reformat or regenerate anything in `packages/`. Those are exact signed bytes.
- Edit `index.json` or add pictures. The team does that when packing.
- Put code, scripts, install commands, links to downloads, keys or personal data in a skill.
- Write a skill that sends, posts, pays, deletes or changes settings by itself.
