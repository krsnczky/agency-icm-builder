# Workflow: {{NAME}}

<!-- Lives at workflows/<name>.md. A multi-step sequence that crosses departments.
     Triggered by phrases in the root router. Each step names its files and its human gate. -->

**Trigger phrases:** "{{what the human types}}"
**Produces:** {{the artifact that leaves the workflow}}

## Steps

### 1. {{step name}}
- Load: {{exact files}}
- Do: {{the work}}
- Write: {{exact output path}}
- Human gate: {{what the human checks before step 2 - or "none"}}

### 2. {{step name}}
- Load: output of step 1 + {{files}}
- Do: {{the work}}
- Write: {{exact output path}}
- Human gate: {{check}}

<!-- Rule of thumb: if no step has a human gate, this is a script, not a workflow.
     If every step needs one, the steps are too big. -->
