# Conventions

The load-bearing conventions in depth. Read this when writing the actual files of a new workspace, or when an audit needs the precise definition of a rule.

Contents: Client triad · Log tags · Pointer file · Load-order contract · Capture protocol · Cross-client patterns · Naming

## The client triad

Three files carry everything an agent needs to work on a client. They answer different questions and have different write disciplines:

| File | Question | Write discipline |
|---|---|---|
| `wiki/hot.md` | What is going on right now? | Rewritten in place as priorities change. Target under 60 lines. Old content is deleted, not archived; history lives in the log. |
| `wiki/log.md` | What happened, in order? | Append-only, one dated entry per event, tagged. Never edited retroactively. |
| `memory/learnings.md` | What have we learned that changes how we work? | Append when a lesson is confirmed, prune when one is invalidated. Rules, not events. |

The test for which file gets a fact: if it will be false in a month, it is `hot.md`. If it is a thing that happened, it is `log.md`. If it changes how the next task should be done, it is `learnings.md`.

Beyond the triad, a client's `wiki/` typically holds slow-changing reference pages: `profile.md` (business background, who the client is, what they sell), `brand.md` (voice, style, do/don't), and per-service state pages (for a PPC agency: one campaign-state page per ad platform; for a design agency: one page per retainer track). Slow pages are loaded per task, not per session.

## Log tags

Every log entry starts with a date and a tag. Tags make the log scannable and greppable:

```
[DECISION]  a choice was made and why
[RESULT]    an outcome, with numbers where possible
[PROBLEM]   something broke or underperformed
[MEETING]   pointer to a meeting note file (full note lives in wiki/meetings/)
[CLIENT]    the client said, asked, or changed something
[CAMPAIGN]  work shipped (rename per service line: [PROJECT], [SPRINT], ...)
```

Adapt the tag set to the agency's services, then keep it stable. A meeting gets a one-line pointer in the log; the full note goes in its own dated file. Logs stay scannable when entries stay short.

## The pointer file

Each client folder carries `.claude/CLAUDE.md`, a pointer file that never holds content. It states the load order for this client: read `wiki/index.md`, read `wiki/hot.md`, read `memory/learnings.md` in full, then load the task-relevant page. Ten lines. When an agent enters the client folder, this file is what orients it.

Why per-client pointer files instead of one global rule: the pointer can carry client-specific exceptions (a compliance page that is mandatory for a regulated client, a platform page that does not exist for a single-platform client) without the global contract growing a special case per client.

## The load-order contract

One file, `system/context-load-order.md`, maps task types to required files:

```
| Task type            | Load                                             |
|----------------------|--------------------------------------------------|
| any client task      | pointer file, wiki/index.md, hot.md, learnings.md |
| report / audit       | + per-service state page, change log              |
| copy / creative      | + brand.md                                        |
| strategy             | + profile.md, cross-client patterns               |
| meeting prep         | + meetings/index.md, last meeting note            |
```

Rules for the contract: name exact files, not "relevant context". Keep one row per task type the agency actually has. A missing required file is flagged to the human, not silently skipped. If a task's row keeps growing, split the task type.

## The capture protocol

At session end, the agent writes back. The protocol:

1. **Determine the session type first.** Client work goes in the client's folder. Workspace infrastructure work (scripts, templates, system files) goes in `system/` changelogs and never in a client log.
2. **Write to the certainly-identified client only.** One session, one client is the happy path. If the session touched several clients, each gets its own entries.
3. **Uncertain attribution goes to quarantine.** If the agent cannot say with certainty which client a note belongs to, it goes to `system/inbox.md` with a flag for the human to file. A guessed client is data corruption; the quarantine costs one human minute.
4. **The write set:** log entry (always), `hot.md` update (if priorities changed), `learnings.md` append (if there was a durable lesson), meeting note file (if there was a meeting).
5. **Automatic, not asked.** The capture happens without the human requesting it. A capture that depends on someone remembering to ask is a capture that stops happening in week two.

## Cross-client patterns

Insight that repeats across clients (a bidding approach that works in every account, an email structure clients respond to) is real agency capital, but it cannot live in any one client folder and must not name clients when generalized. It goes to `system/cross-client-patterns.md`, stated as a rule with the evidence count ("seen in 3 accounts"), stripped of client identity. Strategy tasks load this file; it is the compounding asset of the whole system.

## Naming

- Folders and machine-facing files: kebab-case (`clients/acme-corp/`, `change-log-google.md`).
- Dated files: `YYYY-MM-DD-topic.md`, sorts chronologically for free.
- Template folders: underscore prefix (`_template/`), sorts to the top and reads as "not a real client".
- One language per workspace for structure words (hot, log, learnings); content in whatever language the agency works in. Mixed-language structure words break grep and muscle memory.
