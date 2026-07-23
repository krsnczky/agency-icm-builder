---
name: agency-icm-builder
description: Build or audit an AI-runnable workspace for a client-service agency (marketing, PPC, design, consulting, dev shops). Folder structure does the orchestration - one agent, routing files, per-client folders, session capture. Use when the user wants to (1) set up an agency workspace an AI agent can operate ("build me an agency workspace", "structure my agency for AI"), (2) audit an existing agency folder against these conventions ("audit my workspace", "why does my agent keep mixing up clients"), or (3) restructure a grown-wild agency folder into this shape ("restructure this", "ICM my agency").
---

# Agency ICM Builder

Build workspaces where the folder structure runs the agency. One AI agent, reading the right files at the right moment, coordinates client work across services: the routing files decide what loads, per-client folders guarantee client data never mixes, and a session-end capture protocol turns daily work into durable memory.

This is not theory. The conventions here run a real agency: 15 client folders, 5 service departments, daily production use. Everything below was extracted from that system, then genericized.

The term ICM (Interpretable Context Methodology, folder structure as agent architecture) comes from Van Clief & McDermott, arXiv:2603.16021.

## The ten rules

Every workspace this skill builds or audits follows these. In audit mode, each rule is a check.

1. **The root file routes, it does not teach.** The root `CLAUDE.md` holds identity plus a routing table (task keyword to file path) and nothing else. Content lives in the files it points to. When the root file grows, it is absorbing payload that belongs on a shelf.
2. **Every client is one folder.** Nothing about a client lives outside `clients/<name>/`. This is the no-mixing guarantee: the agent always knows which client it is working on, so client facts always land in the right folder. Cross-client insight goes to a shared patterns file, stripped of client identity.
3. **Three files tell the client's story.** `hot.md` (current state and priorities, rewritten as things change), `log.md` (append-only event history with tags like `[DECISION]` `[RESULT]` `[PROBLEM]`), `learnings.md` (distilled operational rules). Now, what happened, what we learned. An agent reading these three files cold is up to speed.
4. **Load per task, not per session.** A load-order contract (one file in `system/`) says which files each task type needs: a report task loads campaign state, a copy task loads brand voice, a strategy task loads the client profile. The agent loads those and stops. Whole-workspace loads are a bug.
5. **One home per fact.** A link beats a copy. Two hand-maintained copies of the same fact will drift, and drifted copies are worse than no copy, because one of them lies.
6. **New client = copy the template.** `clients/_template/` is the unit of instantiation. Evolution rule: when three real clients independently grow the same folder, the template gains it. One client's extra folder is an exception; three are structure.
7. **State is rewritten, history is appended.** `hot.md` gets edited in place. `log.md` only grows. Never rewrite history to reflect current state; that is what `hot.md` is for.
8. **Sessions end in capture.** Work not written back to the client folder did not happen. The capture is agent-driven at session end: log entry, hot update if priorities shifted, learnings append if there was a lesson. If client attribution is uncertain, the note goes to a quarantine inbox for the human to file. Never guess the client.
9. **Expertise and state live apart.** `departments/` (or `services/`) holds stable know-how: agent instructions and skills that change rarely and apply to every client. `clients/` holds per-client state. `system/` holds workspace infrastructure: policies, the load-order contract, changelogs. A fact that survives every client belongs in a department; a fact about one client never leaves its folder.
10. **Hand-maintained indexes rot: generate or delete.** Any "map of everything" file that a human must remember to update will go stale and start lying. Either a script rebuilds it, or it should not exist. The routing table in the root file is the one exception, and it stays small enough to notice drift.

## Choose a mode

- **Build**: the user describes their agency, you scaffold a workspace.
- **Audit**: an existing workspace, you check it against the ten rules and report. No changes.
- **Restructure**: audit first, then propose a migration map, get approval, migrate.

## Build mode

**1. Interview, a few questions at a time.** Use [assets/templates/questionnaire.md](assets/templates/questionnaire.md). You need: what services the agency sells, the repeating unit of work (campaign, project, retainer month), how many clients, who touches the workspace, what repeats weekly, and where a human checks work before it moves on. Do not ask everything at once; the structure is in how they describe their week.

**2. Map their answers to the skeleton.**

```
agency/
├─ CLAUDE.md                # root router (rule 1)
├─ clients/
│  ├─ _template/            # instantiation unit (rule 6)
│  └─ <client>/             # one folder per client (rule 2)
│     ├─ .claude/CLAUDE.md  # pointer: load order for this client
│     ├─ wiki/              # index, hot, log, profile, brand (rule 3)
│     ├─ memory/            # learnings.md
│     └─ work/              # per-service output folders
├─ departments/             # stable expertise per service line (rule 9)
├─ workflows/               # multi-step sequences that cross departments
└─ system/                  # load-order contract, policies, changelog
```

Rename to their vocabulary (departments/services/pods, wiki/notes, work/deliverables). Names are theirs; the shape is not negotiable.

**3. Scale to their size.** Under 5 clients: skip `workflows/`, collapse departments into a single `playbook/` folder, keep the client triad. The full skeleton earns its weight around 8+ clients or 3+ service lines. Building the full thing for a 2-client freelancer is scaffolding, not architecture; say so.

**4. Fill from templates.** Copy from [assets/templates/](assets/templates/): root CLAUDE.md, client template, department file, workflow file. Write the load-order contract into `system/` naming their actual task types.

**5. Offer enforcement as opt-in.** Conventions hold for weeks; hooks hold forever. If they use Claude Code, [references/enforcement.md](references/enforcement.md) shows how to inject the load-order reminder automatically. Skip it for a first build; add it when the workspace proves out.

**6. Validate with the cold-read test** (below).

## Audit mode

Read-only. Never move, edit, or delete during an audit.

**1. Inventory.** List the tree two levels deep. Note file counts and last-modified where it matters.

**2. Check the ten rules.** For each: pass, or a finding with the exact path and what it costs in practice. The five failures that appear in almost every grown workspace:

- **Payload in the router** (rule 1): root file over ~100 lines, holding protocol text or reference content.
- **Drifting duplicates** (rule 5): the same fact in two hand-edited homes, already diverged. Diff them and show the divergence; it is the most convincing finding.
- **Stale maps** (rule 10): an index file with a last-updated date and missing entries. Name what is missing.
- **Template drift** (rule 6): count folders real clients grew that the template lacks.
- **Orphan buckets**: `tmp/`, `docs/`, `misc/` holding one-off files nothing references.

**3. Report with severities.** For each finding: rule broken, path, cost, smallest fix. Then a verdict: what percentage of the shape is already there, and the three fixes worth doing first. End with what you would *not* change; an audit that flags everything flags nothing.

## Restructure mode

Audit first, in full. Then:

**1. Find the hidden shape.** Most grown agency folders already contain the skeleton informally: some per-client folders, a notes file that is really `hot.md`, a graveyard that is really an archive. Extract what exists; do not bulldoze.

**2. Classify every file**: router / client state / expertise / workspace infra / dead. Dead means stale, duplicated, or superseded; it goes to `_archive/`, never silently deleted.

**3. Propose a migration map.** Old path, new path, role, one line of reasoning. Present the full map and the target tree. **Get explicit approval before moving anything.** This method is built on human gates; honor this one.

**4. Migrate.** Move, then write the router and pointer files, then de-duplicate toward one home per fact, leaving a link where a copy lived if anything might reference it.

**5. Validate with the cold-read test.**

## The cold-read test

Validate any workspace, new or restructured, by walking it as an agent with no memory:

- Open the root file. Can you say which client folders exist and where to go for a named task, from the router alone plus at most two more reads?
- Pick one client. Read only `hot.md`, the last ten lines of `log.md`, and `learnings.md`. Can you state what is going on with this client and what to do next?
- Pick one task type. Does the load-order contract name exact files for it, and does following it give you enough context to start, without loading anything else?
- Ask "where would a session-end note about client X go, and what happens if the client is ambiguous?" The answer must be a path and a quarantine rule, not a judgment call.
- Check any index-like file against reality. If it lies, rule 10 is broken.

If a step fails, fix the structure, not the explanation: move or split files until the walk works.

## Guardrails

- **Do not over-build.** The ladder: a chat, then a saved prompt, then this. A workspace for an agency of one with two clients is usually premature; recommend the client triad alone (`hot.md`, `log.md`, `learnings.md` in one folder per client) and stop there.
- **The structure is not the value.** Tell users this honestly: the folders orchestrate, but the quality of what the agent does still comes from the expertise files they write into departments and the discipline of session capture. An empty skeleton does nothing.
- **Where this loses.** Real-time multi-agent collaboration, many users hitting one workspace concurrently, and pipelines that must branch mid-run without a human. This method is for sequential, human-reviewed, repeatable client work, which is most agency work, but not all of it.

## References

- [references/conventions.md](references/conventions.md) - the client triad in depth, log tags, capture protocol, load-order contract format, cross-client patterns.
- [references/enforcement.md](references/enforcement.md) - making the conventions self-enforcing with Claude Code hooks.
- [assets/templates/](assets/templates/) - root CLAUDE.md, client template, department file, workflow file, questionnaire.
