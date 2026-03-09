# pt-pneuma-ai-context

Pneuma team-level Copilot instructions for the [osinfra-io](https://github.com/osinfra-io) platform teams workspace.

## Overview

This repository is the **pneuma team level** of a three-level GitHub Copilot instruction hierarchy. Instructions here apply to all `pt-pneuma-*` repositories.

```none
Platform   pt-ai-context                   ← universal conventions for all pt-* repos
  └── Team   pt-pneuma-ai-context          ← this repo (applies to all pt-pneuma-* repos)
        └── Repo   .github/copilot-instructions.md   ← in every repo (repo-specific only)
```

## What's in this repo

`.github/instructions/team.instructions.md` — loaded via `COPILOT_CUSTOM_INSTRUCTIONS_DIRS`. Contains conventions shared across all pneuma repositories:

- Three-tier GitHub Actions workflow (sandbox → non-production → production)
- No local state access — plans and applies run exclusively in GitHub Actions
- Regional workspace structure and add-on deployment order (main → regional → subdirectories)

## Setup

Add this repo alongside `pt-ai-context` in your `COPILOT_CUSTOM_INSTRUCTIONS_DIRS`:

```bash
# ~/.zshrc or ~/.bashrc
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS="\
$HOME/repositories/osinfra-io/platform-teams/pt-ai-context,\
$HOME/repositories/osinfra-io/platform-teams/pneuma/pt-pneuma-ai-context"
```
