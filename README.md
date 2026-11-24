# Pavan Skills

Agent skills for understanding code changes, building mental models, and learning through questions.

## Install from GitHub

With Node.js 22.20 or later, run this from any directory to install every skill globally for all agents supported by the installer:

```sh
npx skills@latest add pavanmehta91/pavan-skills -g --all
```

`-g` makes the skills available across projects on your machine. Drop `--all` to choose the skills and agents interactively. If the GitHub repo is private, authenticate with Git or the GitHub CLI first. [Installer documentation](https://github.com/vercel-labs/skills#install-a-skill) · [Private repository access](https://github.com/vercel-labs/skills/blob/main/docs/commands.md#private-repositories)

### Install selected skills

Pass one or more skill names after `--skill`. The installer prompts you to choose the agents.

```sh
npx skills@latest add pavanmehta91/pavan-skills -g \
  --skill html-explainer socratic-learning
```

Add `--agent '*'` to install the selected skills for every supported agent, or use `-a` to name specific agents. This example installs one skill for Codex and Claude Code.

```sh
npx skills@latest add pavanmehta91/pavan-skills -g --skill understand-a-change -a codex -a claude-code
```

## Skills

| Skill | What it does |
| --- | --- |
| [`html-explainer`](html-explainer/SKILL.md) | Builds a standalone HTML explanation of a problem, flow, or change. |
| [`understand-a-change`](understand-a-change/SKILL.md) | Explains code, designs, or changes using clear terminology, concrete scenarios, and visual models. |
| [`socratic-learning`](socratic-learning/SKILL.md) | Guides you through a concept with questions that help you reason it out. |

## Update

Run `npx skills@latest update -g` to update installed global skills. Rerun the install command to pick up newly added skills.
