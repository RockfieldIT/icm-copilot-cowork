# Implementing ICM in Microsoft Copilot Cowork

*A beginner's guide. If you've never used Copilot Cowork, start here.*

This guide shows you how to run an ICM (Interpretable Context Methodology)
workspace inside Microsoft Copilot Cowork. ICM started life with tools like
Claude, but the folders-and-markdown idea at its core maps onto Cowork almost
one-to-one. If anything, Cowork has a few advantages, because it lives inside
Microsoft 365 and can reach your email, Teams, calendar and files directly.

By the end you'll have a working ICM folder in OneDrive that Copilot reads, so
you stop re-explaining yourself every session.

---

## 1. The two ideas in one minute

**ICM** is a way of organising context for an AI assistant using plain folders
and markdown files. Instead of one giant prompt, you split your context into:

- a small **routing file** that says "for this kind of task, read these files";
- one **room** per type of work (email, drafting, reports, and so on), each with
  a file describing how that work is done;
- a **voice file** that captures how you write.

One assistant reads different context at different times. No multi-agent
orchestration, just folders.

**Copilot Cowork** is the agentic side of Microsoft Copilot. It works from a
workspace, reads files from your OneDrive, and can take real actions in Outlook,
Teams, SharePoint and your calendar. That makes it a natural home for an ICM
folder tree.

---

## 2. Why Cowork is a good fit

- **It reads your files from OneDrive.** Your ICM folder lives in OneDrive, and
  Copilot can load it on demand. Your context persists between sessions.
- **It has native M365 access.** Rooms like email triage or meeting prep get
  easier, because Copilot already sees your inbox and calendar. No extra
  connectors to wire up.
- **It is markdown-first.** Everything you write is plain text you can edit in
  any editor, version in Git, and read at a glance.

The trade-offs are small and easy to handle, and we cover them below.

---

## 3. What you need

- A Microsoft 365 account with Copilot, enrolled in the Frontier Progam (and Cowork available to you).
- OneDrive for Business.
- A text or markdown editor you like. VS Code is handy because it previews markdown, but
  Notepad works too.
- About an hour for your first setup. You'll add to it over time.

---

## 4. The structure

A minimal ICM workspace looks like this:

```
ICM/
├── setup.md            # routing file: identity + folder map + routing table
├── soul.md             # your voice and tone
├── _config/            # shared references, loaded only where needed
│   └── style.md
├── email/
│   └── CONTEXT.md      # one room = one folder + one CONTEXT.md
├── drafting/
│   └── CONTEXT.md
└── reports/
    └── CONTEXT.md
```

Three layers, and the discipline matters:

1. **`setup.md` (root only).** Your routing file. Keep it short, ideally under
  50 lines. It holds two lines of identity, a folder map, a routing table, and
  any standing rules. It is not a knowledge document.
2. **`CONTEXT.md` (one per room).** Describes the work: the process, what good
  looks like, the constraints. Roughly 80% about the work, 20% about how the
  assistant should behave.
3. **`soul.md` and `_config/`.** Your voice, plus stable references like a style
  guide or brand sheet. Loaded only by the rooms that need them.

The golden rule: `setup.md` lives at the root only. Sub-folders never get their
own routing file. They use `CONTEXT.md`.

---

## 5. Build it, step by step

### Step 1 — Create the folder in OneDrive

Make a folder in OneDrive for Business called `ICM` (or whatever you like).
Everything lives here so Copilot can find it.

### Step 2 — Write `setup.md`

Start lean. Copy this and edit:

```markdown
# [Your name] — Copilot Workspace (ICM)

Routing file. Read this first, then load the room and any "also load" files
the task needs.

## About me
[Name], [role] at [organisation]. [One line on what the organisation does.]

## Folder map
ICM/
├── setup.md
├── soul.md
├── _config/style.md
├── email/CONTEXT.md
├── drafting/CONTEXT.md
└── reports/CONTEXT.md

## Routing
| Task | Room | Also load |
|------|------|-----------|
| Triage or reply to email | email/CONTEXT.md | soul.md |
| Write a document or proposal | drafting/CONTEXT.md | soul.md, _config/style.md |
| Write a report or update | reports/CONTEXT.md | soul.md |

## Rules
- Read this file first; load only the room(s) the task needs.
- Never send anything on my behalf without my explicit approval.
- [Add your own standing rules.]
```

### Step 3 — Write `soul.md`

This is how the assistant writes as you. Be specific. Examples beat adjectives.

```markdown
# Voice

## How I sound
[e.g. Plain, direct, friendly. Short sentences. No jargon.]

## Rules
- [e.g. Greeting: first name only, no "Dear".]
- [e.g. Sign-off: "Kind regards," for external, "Thanks!" for quick internal.]
- Never write: [your list of phrases you can't stand].

## Example
Good: [paste a couple of lines that sound like you]
Bad: [paste something that definitely doesn't]
```

### Step 4 — Write your first room

Pick the work that causes you the most friction today. For most people that's
email. Keep it to the job, not your life story:

```markdown
# Email — Triage and Replies

## What this room does
[One paragraph: what you want help with in your inbox.]

## Process
1. [Step one, e.g. flag anything that needs action today.]
2. [Step two, e.g. summarise the important non-urgent items.]
3. [Step three, e.g. list the rest for me to classify.]

## What good looks like
- [3-5 bullets describing an ideal result.]

## Constraints
- Never send a reply without my approval.
- [Anything the assistant must never do here.]
```

### Step 5 — Start small

One or two rooms is plenty. Add more from real use, not in advance. A room you
never use is just tokens.

---

## 6. How Copilot actually uses your files

There are two ways to put your ICM context to work, and you can grow from one to
the other.

### Tier 1 — Reference your files (works today)

In a Copilot Cowork or Copilot Chat session, point Copilot at the files for the
task. In Copilot Chat you do this by typing `@` and selecting the file; in
Cowork you can simply ask it to read the folder. A typical opener:

> Read `setup.md`, `soul.md` and `email/CONTEXT.md`, then triage my inbox and
> tell me what needs attention today.

Copilot loads the context and works in it. No setup beyond having the files in
OneDrive.

### Tier 2 — Build a dedicated agent (best experience)

Once a room is stable, turn it into a declarative agent with Copilot Studio's
Agent Builder. Paste the room's `CONTEXT.md` (and your `soul.md`) into the
agent's instructions, point its knowledge at the relevant OneDrive folder, and
publish. After that you just open the agent and it is already in context. No
file referencing each time.

Start with Tier 1 for a week. Move the rooms you use daily to Tier 2.

---

## 7. Make ICM your default (the "Claude Projects" question)

If you're coming from Claude, you'll be used to **Projects**: you pick a project
and every chat inside it automatically loads that project's instructions and
knowledge. Copilot Cowork has no feature by that exact name, but you can get the
same result: every session starts inside your ICM structure, with no
re-referencing.

There are two ways to do it. Choose based on whether ICM is your *single* main
workspace or *one of several* contexts you switch between.

### Option 1 (recommended) — set ICM as your standing default

Cowork reads a personal instructions file at the start of every session:
**`copilot-instructions.md`**, kept in your Cowork folder in OneDrive (often
`Documents/Cowork`). It's the equivalent of a global custom-instructions box that
applies to *every* conversation.

Add a short anchor that points Copilot at your ICM workspace:

```markdown
# Personal Copilot instructions

## Default workspace — ICM
For any work task, treat the ICM workspace in my OneDrive as the operating
context. Before starting, read `ICM/setup.md` and follow its routing table:
load only the room (and any "also load" files) the task needs.

- Never send email or messages on my behalf without explicit approval.
- [add any other standing rules]
```

That's the whole job. From then on, every Cowork session is anchored in ICM
automatically, with no `@`-referencing and no reminders. Keep the file short and
declarative; it's an instruction, not a knowledge document.

**Best when:** ICM is your single main workspace and you want it loaded
everywhere by default.

### Option 2 (alternative) — an `icm` skill you switch on

The other way is a **personal skill**: a small folder with a `SKILL.md` file,
kept in your Cowork folder, that you trigger on purpose, by typing `/icm` or by
letting its description match what you're doing. The `SKILL.md` tells Copilot to
load the workspace and work in its structure:

```markdown
---
name: icm
description: Load and work inside my ICM workspace. Use when I type /icm, or
  when I'm doing my regular work (email, drafting, reports, and so on).
---

Read `ICM/setup.md` in my OneDrive and follow its routing table. Load only the
room and "also load" files the task needs. Stay within the ICM structure until
I say otherwise.
```

**Best when:** you keep more than one context and want to *step into* ICM
deliberately, rather than have it always on.

### How the two differ

|     | Option 1 — `copilot-instructions.md` | Option 2 — `icm` skill |
| --- | --- | --- |
| **When it loads** | Always, every session, automatically | On demand, when you type `/icm` or its description matches |
| **The gesture** | None — it's simply on | Deliberate, like picking a Project |
| **Scope** | Global default for all your Cowork work | One context among several you can switch between |
| **Best for** | ICM is your single main workspace | You juggle several workspaces or contexts |
| **Closest Claude analogue** | Global / project custom instructions | "Select the project" before you start |

The two aren't mutually exclusive. A common setup is Option 1 as a quiet default,
with an `/icm` skill kept around for the odd session that began somewhere else.
And when you want this same anchoring to follow you across *all* of Microsoft 365,
not just Cowork, promote the room to a Copilot Studio agent (see Section 6, Tier 2).

---

## 8. Keep token use down

Cowork reads your files, so smaller files mean lower cost and sharper focus.
A few habits keep things lean:

- **Routing file stays tiny.** `setup.md` is a map, not an encyclopaedia.
- **Load only what the task needs.** That's the whole point of the routing
  table. Don't load every room every time.
- **Describe the work, not the assistant's personality.** Personality lives in
  `soul.md`, once.
- **Tighten as you go.** Cut repeated examples, keep the rules. One good example
  beats five. When I rebuilt my own workspace for Cowork, tightening the markdown
  this way cut total size by about 40% with no loss of instruction.
- **Tables over prose** for anything structured (routing, reference lists,
  patterns). They're shorter and the assistant reads them cleanly.

---

## 9. Adapting from a Claude-style ICM workspace

If you already run ICM elsewhere, the move to Cowork is mostly mechanical:

- Rename your root `CLAUDE.md` to `setup.md` and rewrite the "how to use" lines
  for Copilot (reference files in chat, or build an agent).
- Swap any third-party connectors for Copilot's native M365 access. Email,
  calendar and Teams rooms get simpler.
- Remove machine-specific file paths. Your files live in OneDrive now, so use
  relative paths inside the tree.
- If a step assumes a specific assistant by name, make it generic.

Everything else, your rooms, your voice file, your references, carries straight
across.

---

## 10. Common pitfalls

- **One giant file.** If `setup.md` grows past a page, you've put knowledge in
  the router. Move it into a room.
- **Vague rooms.** "Help me with email" produces generic output. Spell out the
  process and the constraints.
- **A routing file in every folder.** Only the root has `setup.md`. Sub-folders
  use `CONTEXT.md`.
- **Setting and forgetting.** These are living documents. When Copilot does
  something you didn't want, add a constraint. The files get better as you use
  them.

---

## 11. Maintenance

- Edit a file whenever you notice a gap. The change takes effect next session.
- Add a room when a new type of work becomes regular, not before.
- Review your agents now and then. Models update, and a tightened instruction
  set keeps them on track.

---

## Quick-start checklist

- [ ] Create an `ICM/` folder in OneDrive.
- [ ] Write a lean `setup.md` (identity, folder map, routing table, rules).
- [ ] Write `soul.md` with real examples of your writing.
- [ ] Add one room (`CONTEXT.md`) for your most painful task.
- [ ] In Copilot, reference `setup.md` + the room and try a real task.
- [ ] Make ICM your default: add an anchor to `copilot-instructions.md` (or create an `/icm` skill).
- [ ] Once it's solid, build a Copilot Studio agent for that room.
- [ ] Add rooms over time. Keep every file lean.

---

*ICM (Interpretable Context Methodology) is a community approach to context
engineering. Credit for the methodology: [add attribution/link]. This guide
covers the Copilot Cowork implementation specifically.*
