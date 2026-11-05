# Skill mechanics

The skill-specific branch of [`writing-for-agents`](SKILL.md): what changes when the document is a skill (frontmatter, the invocation choice, and router skills). Everything else about writing it is the universal reference in `SKILL.md`.

## Invocation

Pi loads skill instructions through file reads. Distinguish **discovery** (the advertised description), **explicit invocation** (the user's `/skill:name` command), and **reference** (an agent follows a file pointer). `disable-model-invocation` controls discovery, not filesystem access.

Two discovery choices, trading the two loads:

- A **model-invoked** skill keeps a `description`, so the agent can fire it autonomously, and other skills can reach it. You can still type its name: model-invocation always _includes_ user reach; a description only ever adds agent discovery, never removes the human's. The description is the skill's top-level context pointer, forced to stay loaded at all times: permanent context load in exchange for discoverability. A model-invoked skill whose content is all reference is also one home for shared reference: another skill can invoke it, so reference needed by several skills lives in one place. Mechanics: omit `disable-model-invocation`, and write a model-facing description carrying the trigger branches (the pointer-writing rules in `SKILL.md` apply in full).
- A **user-invoked** skill is hidden from automatic model selection. The user invokes it with `/skill:name`; an agent can still read its `SKILL.md` through an explicit pointer from the user or another document. Its description pays no automatic-discovery load, but the human or pointer must supply reach. Mechanics: set `disable-model-invocation: true`; keep the required `description` as a human-facing one-line summary, trigger lists stripped.

Pick model-invocation when the agent needs automatic discovery. If explicit user commands or document pointers provide sufficient reach, use `disable-model-invocation: true`.

Shared reference may live in a skill reached by an explicit pointer, regardless of its discovery flag, or in a plain reference file. Keep one authoritative copy; choose a plain file when the material needs no independent skill invocation.

## Splitting by invocation

The invocation cut of splitting (the sequence cut lives in `SKILL.md`): split off a model-invoked skill when a distinct leading word should trigger automatic discovery (a trigger word you actually use in your prompts). A branch reached only through another document can instead use a file pointer. You pay context load for each advertised description, so independent discovery has to be worth it.

## Router skills

When user-invoked skills multiply past what you can remember, use a **router skill**: one user-invoked skill that names the branches and when to reach each. For user choice, list `/skill:name` commands; for agent execution, provide `SKILL.md` pointers and require reading the selected branch. Resolve relative pointers from the router's directory, not the project cwd.
