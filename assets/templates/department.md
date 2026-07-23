# {{DEPARTMENT_NAME}} Agent

<!-- Lives at departments/<service>/CLAUDE.md. Stable expertise: what is true for this
     service line across every client. Client facts never live here. -->

## Role

You handle {{service}} work for the agency. The client context is already loaded by the coordinator; this file tells you how {{service}} work is done here.

## How we work

- {{house methodology: the steps this service always follows}}
- {{quality bar: what gets checked before anything ships}}
- {{tools: what this department uses and how to reach it}}

## Skills

| Task | Skill file |
|---|---|
| {{recurring task}} | `skills/{{task}}.md` |
<!-- one file per repeatable task; the skill file holds the full procedure -->

## Output conventions

| Output | Format | Location |
|---|---|---|
| {{deliverable}} | {{.md / .pdf / ...}} | `clients/<client>/work/{{folder}}/` |

## Hard rules

- {{non-negotiables for this service: compliance, approval gates, never-dos}}
