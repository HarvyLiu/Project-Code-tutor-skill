# Part-by-Part Tutor Skill

A hints-first micro-step coding tutor skill for AI agents (built for OpenCode, works anywhere `SKILL.md` skills load).

Born from a real session building **Julius Ceasar** — a Python Slack Bolt bot (`/caesar`, `/rot13`, `/base64`, Socket Mode, Nest deploy). The student wrote all code; the agent guided part-by-part. This skill bottles that method so any agent can reuse it.

## Triggers

Load this skill when the user says any of:

- `part-by-part`, `part by part`
- `hints first`, `just hints`
- `teach me`, `I'm a beginner`
- `pretend I didn't see the code`
- `check my files`
- `why does X work`, `what does async do`, `I don't get OOP`
- `just give me the code for this part`

## How it teaches (the loop)

1. **Teach the whole map first.** Before any code: 3–6 numbered steps for the full logic + a glossary of unfamiliar functions. Never jump straight to tasks.
2. **Assign ONE hint-task.** 2–3 lines max with `TODO` blanks, not the solution.
3. **User attempts → agent checks.** Read the files, report bugs as `file:line` with hint-questions, not fixes.
4. **Second failure on the same part →** show that part's code for the user to paste, teach each line, then give the next part's hint.
5. **Verify.** One run command per part with expected output (e.g. `python -c "from ciphers.b64 import b64_encode; print(b64_encode('hello'))"` → `aGVsbG8=`).

## Operating rules

- **User builds, agent guides.** Read-only guidance by default. Only edit/write files when the user explicitly says so *and* the mode permits it (plan mode is read-only and overrides).
- **Auto-check files every turn.** `read` relevant files first; never print `.env` secrets (confirm key presence only).
- **Short and concise.** One concept per turn, everyday analogies (cookie cutter = class, cookie = instance).
- **Reference code as `file:line`.**

## Concepts covered

`dict.get` vs `[]` · truthiness + `.strip()` · `split(" ", 1)` vs `rsplit` · `except ValueError` vs `except Exception` · `if __name__ == "__main__"` guard · OOP minimal (class/instance/method) · sync vs async (stay sync for CPU work; `ack()` first solves concurrency) · `str` vs `bytes` + base64 `validate=True` · Windows UTF-16 `echo >` trap + fix · Slack Socket Mode 2-token flow + reinstall dance.

## Install

Skill folder name must stay `part-by-part-tutor` (matches `name` in `SKILL.md` frontmatter).

**Global (all projects):**
```bash
git clone https://github.com/HarvyLiu/Project-Code-tutor-skill part-by-part-tutor
# move it to ~/.config/opencode/skills/part-by-part-tutor
# (Windows: %USERPROFILE%\.config\opencode\skills\part-by-part-tutor)
```
Then quit and restart OpenCode so the skill loader picks it up.

**Per project:**
```bash
# copy SKILL.md to .opencode/skills/part-by-part-tutor/SKILL.md
```

## Repo layout

```text
SKILL.md   # the skill (frontmatter name: part-by-part-tutor)
README.md  # this file
```

## Reference session

Julius Ceasar bot: `app.py` slash-handler 5-step shape (`ack()` → read `command["text"]` → validate/split → pure cipher function → `respond()`), `ciphers/` pure functions, `SocketModeHandler` for 24/7 on Nest (Debian 13, systemd). ROT13 taught as `caesar(13, text)` reuse; base64 taught as the bytes dance (`str --encode--> bytes --b64encode--> bytes --decode--> str`).
