# Model-invoked vs user-invoked

Every `SKILL.md` in this repo is a skill. The one axis that splits them is **invocation** — who can reach it:

- **User-invoked** — reachable **only by the human typing its name**. Set
  `policy.allow_implicit_invocation: false` in `agents/openai.yaml`. The
  `description` is **human-facing**: a one-line summary read by a person
  browsing slash-commands. Strip trigger lists ("Use when the user says…").
- **Model-invoked** — reachable by **model or user**. The default: omit the
  `policy` block from `agents/openai.yaml`. The `description` is
  **model-facing** and keeps rich trigger phrasing ("Use when the user wants…,
  mentions…, asks for…") so auto-invocation fires. The test for whether a skill
  should stay model-invoked: _could the model usefully reach for this
  autonomously?_ (Reuse is the reason to extract a skill, not the test.)

This mirror defines the OpenAI invocation contract only. Upstream remains the
authority for other harnesses. In OpenAI, nothing but the human can fire a
user-invoked skill, and no other skill can reach it. A user-invoked skill may
invoke model-invoked skills, but it can never reach another user-invoked skill.

Every selected skill carries an `agents/openai.yaml` beside its `SKILL.md`. It
holds Codex UI metadata — `interface.display_name` and
`interface.short_description` for the skill picker — and, for user-invoked
skills, `policy.allow_implicit_invocation: false`.

Bucket `README.md`s and the top-level `README.md` group entries into **User-invoked** and **Model-invoked**.

## Dependencies between them

Dependencies are expressed as **`/skill`-style prose invocation** ("Run the `/grilling` skill"), not deep `../other-skill/FILE.md` cross-references. Shared reference docs live inside the skill that owns them; other skills reach that material by invoking the skill, not by linking across folders.

## Passive vs active domain work

Merely _reading_ `CONTEXT.md` for vocabulary is a one-line prose pointer, not the `domain-modeling` skill. Only the active build/sharpen discipline (challenge terms, edge-case scenarios, write ADRs, update `CONTEXT.md` inline) is `domain-modeling`.
