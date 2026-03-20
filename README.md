# BMCFADS Agent Skills Authoring

> [!NOTE]
> If you are looking for complete, installable BMCFADS Agent Skills, see [agent-skills](https://github.com/bmcfads/agent-skills).

This repo is for authoring and experimenting with Agent Skills. It is used by only BMCFADS (for now) to design, iterate on, and prepare skills for publication to the public. Once a skill is ready for the public, it is pushed to the [agent-skills](https://github.com/bmcfads/agent-skills) catalog repository.

## Contributing
If you are contributing to this repository, start with:

[CONTRIBUTING.md](/CONTRIBUTING.md) — authoring conventions and workflow

## Repository structure

```
agent-skills-authoring/
├─ .github/
|  └─ workflows/    # CI/CD pipelines
├─ .published/      # Immutable snapshots of published skills
├─ releases/        # Skills approved and ready to publish
├─ skills/          # Active authoring (WIP skills)
├─ templates/       # Authoring templates (never published)
├─ tools/           # Validation and release tooling
```

## Skill lifecycle overview

Skills are authored and validated in this repository before being published to the catalog repository.

Ownership transfers at publication time. After a skill is published, all ongoing work happens in the catalog repository.
