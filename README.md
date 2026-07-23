<!-- banner goes here when ready:
<p align="center">
  <img src="assets/banner.png" alt="Agency ICM Builder - folder structure as agent architecture for agencies" width="760">
</p>
-->

<h1 align="center">Agency ICM Builder</h1>

<p align="center">
  Build, audit, or restructure an AI-runnable workspace for a client-service agency.<br>
  Folder structure does the orchestration. Packaged as a <a href="https://claude.com/claude-code">Claude</a> <strong>skill</strong>.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square" alt="License: MIT"></a>
  <img src="https://img.shields.io/badge/Claude-skill-CD5334.svg?style=flat-square" alt="Claude skill">
  <img src="https://img.shields.io/badge/version-1.0-CD5334.svg?style=flat-square" alt="Version 1.0">
</p>

## Contents

- [The idea](#the-idea)
- [What it does](#what-it-does)
- [The shape it builds](#the-shape-it-builds)
- [Install](#install)
- [Layout](#layout)
- [Works well with](#works-well-with)
- [Credits](#credits)

## The idea

One agent, routing files, one folder per client, and a session-end capture protocol that turns daily work into durable memory. No framework, no database, no orchestration code. Markdown files in folders, arranged so that an agent always knows where it is, what to load, and where its work goes.

Extracted from a real system: these conventions run a marketing agency with 15 client folders and 5 service departments in daily production, built and hardened over months of real client work. This skill is the genericized version.

## What it does

Three modes:

- **Build** - interviews you about your agency (services, clients, the repeating unit of work, where humans check things), then scaffolds the smallest workspace that carries it. A 2-client freelancer gets three files per client; a 15-client shop gets the full skeleton.
- **Audit** - checks an existing workspace against ten rules and reports findings with paths, costs, and smallest fixes. Read-only.
- **Restructure** - audit first, then a migration map for your approval, then migration. Your grown-wild folder usually already contains the structure informally; it gets extracted, not bulldozed.

## The shape it builds

```
agency/
├─ CLAUDE.md                # routes tasks to files; holds no content itself
├─ clients/
│  ├─ _template/            # new client = copy this
│  └─ acme-corp/
│     ├─ .claude/CLAUDE.md  # load order for this client
│     ├─ wiki/              # hot.md (now), log.md (history), profile, brand
│     ├─ memory/            # learnings.md (distilled rules)
│     └─ work/              # deliverables
├─ departments/             # stable expertise per service line
├─ workflows/               # multi-step sequences with human gates
└─ system/                  # load-order contract, capture rules, changelog
```

The core guarantees:

- **Clients never mix.** Everything about a client lives in its folder; uncertain attribution goes to a quarantine inbox instead of a guessed folder.
- **Load per task, not per session.** An explicit contract says which files each task type needs. The agent loads those and stops.
- **Sessions end in capture.** Log entry, state update, lesson learned - written back automatically, so week 30 of the workspace knows what week 1 learned.
- **State is inspectable.** Any human can open `hot.md` and `log.md` and see exactly where a client stands. No dashboard, no vendor.

## Install

**Claude Code:** copy this folder to `~/.claude/skills/agency-icm-builder/` (or `.claude/skills/agency-icm-builder/` inside a project), then say "build me an agency workspace", "audit my workspace", or "restructure this for agents".

**Claude apps:** zip this folder's contents and upload via Settings, then Capabilities.

## Layout

```
agency-icm-builder/
├─ SKILL.md                 the method: ten rules, three modes, the cold-read test
├─ references/
│  ├─ conventions.md        client triad, log tags, capture protocol, load-order contract
│  └─ enforcement.md        making conventions self-enforcing with Claude Code hooks
└─ assets/templates/        root CLAUDE.md, client template, department, workflow, interview
```

## Works well with

[agency-memory-kit](https://github.com/krsnczky/agency-memory-kit) - a Claude Code plugin from the same production system that automates the memory side: session briefing hooks, context injection, and weekly memory consolidation. This skill builds the structure; the kit keeps it alive.

## Credits

The term ICM (Interpretable Context Methodology - folder structure as agent architecture) comes from Van Clief & McDermott, [arXiv:2603.16021](https://arxiv.org/abs/2603.16021). The conventions in this skill were developed independently in production and share the methodology's core claim: for sequential, human-reviewed client work, the filesystem is the framework.

MIT licensed.
