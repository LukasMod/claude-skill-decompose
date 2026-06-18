# claude-skill-decompose

A Claude skill for decomposing React and React Native components into cleaner boundaries.

## Files

- `SKILL.md` — main skill instructions
- `GLOSSARY.md` — supporting terminology used by the skill

## What it helps with

Use this skill when you want to improve component architecture instead of patching symptoms. It focuses on:

- cleaner ownership
- smaller components
- narrower subscriptions
- custom hook extraction
- simple props instead of broad object passing
- React Compiler-friendly structure

## Local installation

Copy this skill directory into your local Claude skills directory, for example:

```bash
cp -R claude-skill-decompose ~/.claude/skills/decompose
```

Then invoke it from Claude Code with:

```text
/decompose
```
