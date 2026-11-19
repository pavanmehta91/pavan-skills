---
name: understand-a-change
description: Automatically use when the user asks to understand something, says "help me understand", "explain this", "teach me", or expresses confusion about a concept, system, code, PR, diff, design, or feature. Build understanding through clear terminology, concrete scenarios, and visual models. Apply to learning requests, not requests solely to implement, debug, or review correctness.
---

# Understand a Change

Build a mental model the user can explain in their own words. For a change, anchor teaching in the actual change and product behavior. For a general topic, use a concrete example and omit change-specific steps that do not apply. Teach concepts without requiring language syntax unless the user wants it.

## Establish the facts before asking reasoning questions

- Use the supplied PR, diff, design, screenshots, and available source. Inspect only the surrounding code needed to explain the change. Prefer read-only access; do not switch branches or modify the project to teach it.
- Separate what already exists, what this change adds or changes, and what remains unchanged. Distinguish implemented behavior from planned behavior, especially storage or evaluators that are not connected to production flows yet.
- Start with a short explanation of the problem and intended outcome. Assess understanding with a focused question or the user's own description; do not ask them to reason about entities you have not defined.
- Respect their desired depth. If they exclude syntax, use product examples, plain-language steps, and small data sketches instead of language tutorials.
- Label inferences and unknowns. Explain observed implementation choices separately from your interpretation of why the author chose them.

## Establish a stable vocabulary and model

- Define the essential entities using one concrete example. Use the same names throughout. If source code, the UI, and the user use different names, map those names explicitly once.
- Do not introduce extra synonyms such as “shared policy” for an already established “ruleset” without explaining that they are the same entity.
- For each relevant entity, explain what it represents, who owns it, and its relationship to other entities. Clarify one-to-one versus one-to-many when it affects behavior.
- For storage questions, distinguish an ID reference from a copied value or an embedded document. Show what is actually stored in each parent; do not imply that selecting a referenced object copies it.
- Connect the model to the actual UI when asked. Verify where configuration happens from source or available evidence; distinguish existing UI from UI deferred to another change.

## Teach WHY → WHAT → HOW, one uncertainty at a time

- Explain why the capability is needed, what concepts solve the problem, then how this change connects them.
- Give direct factual answers before asking a Socratic question. Ask at most one thought-provoking question per response, and only when it advances understanding.
- Use the user's concrete scenario before inventing another. Trace inputs, decisions, and outcomes with familiar software examples.
- If two things sound similar, first establish whether they govern the same decision. Separate timing, conditions, actions, and configuration sources before discussing precedence.
- Explain precedence only among competing settings for the same decision. Explicitly distinguish replacement, merging, inheritance, and independent checks that both apply.
- When applicable, distinguish absent configuration, an explicitly empty configuration, and a disabled rule. Verify their behavior instead of assuming they mean the same thing.
- When the user is confused, stop adding concepts. Identify the specific ambiguity, redraw the smallest useful model, and answer it plainly. Avoid repeating the same abstract explanation at greater length.

## Use visuals when relationships become hard to hold in mind

- Use a small entity diagram for ownership and references, a timeline for when checks happen, or separate decision flows for independent precedence rules.
- Put each visual next to the explanation it supports. Prefer meaningful product labels; include code identifiers only when they help connect the explanation to evidence.
- When the user requests an HTML explainer, create and open a focused, self-contained HTML file with a light color scheme. Use diagrams and, when useful, a small interactive scenario with explicit assumptions. Mark illustrative behavior and future integration clearly.
- Keep visual output portable: use available file tools and an OS launcher or artifact viewer, without depending on a particular assistant's plugins. Verify the artifact's structure and interactive logic as appropriate.

## Verify understanding without turning it into an exam

- Invite the user to explain the model in their own words or predict one concrete outcome once the basic facts are clear.
- Preserve the correct parts of their explanation and correct only the mistaken relationship or assumption. Use a counterexample when it makes the distinction clearer.
- Increase difficulty only after the current model is understood. Finish a substantive learning sequence with one practical scenario or exercise; skip repeated quizzes when the user is simply confirming or closing the discussion.
- Keep responses concise. Do not force a full lesson, report, review, or implementation when the user asks a narrow follow-up. Learning alone does not authorize posting comments, changing code, or creating tickets.
