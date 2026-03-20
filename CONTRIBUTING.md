#  Contributing to Agent Skills Authoring

This document defines the authoring workflow, lifecycle rules, and CI/CD invariants that keep skill ownership clear and releases predictable.

Contents:

- [Who this repository is for](#who-this-repository-is-for)
- [Core principles](#core-principles)
- [Skill lifecycle](#skill-lifecycle)
- [Authoring a new skill](#authoring-a-new-skill)
- [Promoting a skill for release](#promoting-a-skill-for-release)
- [Making changes after publication](#making-changes-after-publication)
- [Directory semantics](#directory-semantics)
- [Resources](#resources)
- [Examples](#examples)

---

## Who this repository is for
> [!NOTE]
> If you are looking to install or use skills, this is not the right repository. See the [agent-skills](https://github.com/bmcfads/agent-skills) catalog repository instead.

- BMCFADS for authoring new Agent Skills

## Core principles

These rules are non‑negotiable and explain why the workflow looks the way it does:

- **Single source of truth** — a skill is owned by exactly one repository (authoring or catalog) at a time, and lives in only one directory at a time
- **Explicit release intent** — nothing is published implicitly
- **One‑way lifecycle** — skills only move forward
- **No mirroring** — authoring and catalog repositories are never edited in parallel

## Skill lifecycle

1. Copy template from `templates/skill/` to `skills/<skill-name>`
2. Develop skill in `skills/`
3. Move skill to `releases/` when ready for the public
4. CI/CD:
    1. Push skill from `releases/`to catalog repository experimental directory
    2. Move skill from `releases/` to `.published/`
    3. Delete skill from `skills/` (if required)

### Lifecycle invariants

At any point in time:

- A skill may exist in only one of: `skills/`, `releases/`, or `.published/`
- CI/CD publishes only from `releases/`
- Nothing ever moves backward in the lifecycle

## Authoring a new skill

1. Start from a template, e.g. `templates/skill/`
2. Copy the template into `skills/<skill-name>/`
3. Develop and iterate freely
4. Validate locally using tooling in `tools/`

Everything under `skills/` is considered experimental by default.

### Naming convention

Skill names must be imperative verb phrases in kebab‑case (e.g. `do-something-cool`). Do not include prefixes such as `bmcfads-`.

### Versioning

Skills in this repo will have the verion `0.1.0`. This version only needs to be updated after the skill has been made available by being pushed to the catalog repository, and changes are subsequently made to the skill.

The major version stays at `0` until the skill has been moved out of the catalog experimental directory.

The exception is if you want to develop a new major version (e.g. v2) of a skill inside this repository. When pushed to the catalog experimental directory, it will have the corresponding major version (e.g. `2.0.0`).

### Validation

**TODO:** add directions to validate with [skill-ref](https://github.com/agentskills/agentskills/tree/main/skills-ref).

## Promoting a skill for release

Promotion is an explicit human decision followed by automated execution.

### Manual promotion

When a skill is ready to be published, move the skill from `skills/` to `releases/`:

```
mv skills/<skill-name> releases/<skill-name>
```

Commit the change:

```
git commit -m "Promote <skill-name> to release"
```

This action signals release intent.

### [Future] CI/CD Behavior

When a change appears under `releases/`, CI/CD will:

- Validate the skill structure and metadata
- Publish the skill to the catalog repository experimental directory
- Move the skill from `releases/` to `.published/`
- Remove the corresponding skill from `releases/`
- Remove the corresponding skill from `skills/` (if required)

After this step, the skill must not exist in `skills/` or `releases/`, only `.published/`.

CI/CD is the only actor allowed to write to `.published/`, and publication marks the point at which all further work continues in the catalog repository.

## Making changes after publication

Once a skill has been published:

- Ownership transfers to the catalog repository
- All future changes happen via PRs in the catalog repository
- This repository must not continue editing the skill
- There is no syncing, mirroring, or back‑porting

### Bug fixes and improvements

- Make changes directly in the catalog repository
- Follow the contribution guidelines in the catalog repository

### Major redesigns (v2 skills)

If a large redesign is required:

- Create a new skill in this repository (e.g. `skills/do-something-cool-v2/`)
- Iterate freely in this repository
- Promote the skill to releases to publish it as a new experimental skill in the catalog repository (drop the `-v2` from the skill name)
- Do not reuse or resurrect `.published/` snapshots

```
mv skills/do-something-cool-v2 releases/do-something-cool
```

When the new major version is ready to move out of the experimental stage:

- Move the previous version of the skill to the catalog repository deprecated directory
- Move the new version of the skill from catalog respository experimental directory to the catalog respository skills directory

## Directory semantics

`templates/`

- Starting point(s) for new skills
- Used during authoring only
- Never published

`skills/`

- Active work-in-progress skills
- May change, break, or be renamed at any time
- Not published directly

`releases/`

- Skills that are ready to be published
- Skills are moved here via an intentional manual promotion
- Treated as immutable release candidates
- CI/CD publishes only from this directory

`.published/`
> [!WARNING]
> Do not edit anything under `.published/`

- Historical snapshots of skills that were published from this repository
- Managed automatically by CI/CD
- Read-only
- No longer owned by this repository

`tools/`

- Internal authoring and release tooling
- Used for validation, checks, and automation support
- Not published and not consumed by developers
- May change independently of skill content

## Resources
- [`agent-skills` Catalog Repository](https://github.com/bmcfads/agent-skills)
- [Agent Skills Specification](https://agentskills.io/specification)
- [`skills` CLI Tool](https://www.npmjs.com/package/skills)
- [`skills-ref` CLI Tool](https://github.com/agentskills/agentskills/tree/main/skills-ref)

## Examples

- [The Agent Skills Directory](https://skills.sh/)
- [Anthropic Skills](https://github.com/anthropics/skills)
- [OpenAI Skills](https://github.com/openai/skills)
- [Vercel Skills](https://github.com/vercel-labs/agent-skills)
