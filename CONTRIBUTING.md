# Contributing to NexFort

## Branches

- `main` contains reviewed, usable changes.
- `develop` is the integration branch for upcoming work.
- Create work branches from `develop` using `feature/<name>`, `fix/<name>`,
  `refactor/<name>`, or `docs/<name>`.
- Keep each branch focused on one independently reviewable change.

Do not commit directly to `main`. Merge work branches into `develop` through a
pull request. Promote `develop` to `main` through a separate release pull
request after verification.

## Preparing a change

1. Fetch `origin` and `upstream`.
2. Update `develop` before creating a work branch.
3. Make small commits with concise, plain-language subjects.
4. Update `CHANGELOG.md` for user-visible behavior.
5. Build only the Debug configuration and launch the affected workflow.

Follow `AGENTS.md` and `REVIEW.md` for repository-specific C++, UI,
localization, serialization, and formatting rules.

## Pull requests

Every pull request must include:

- the problem and intended outcome;
- a concise summary of the implementation;
- test steps and actual results;
- platforms tested;
- screenshots or recordings for visible UI changes;
- compatibility, privacy, performance, and migration risks;
- related issue or project-plan item.

Keep generated files, unrelated formatting, and opportunistic cleanup out of
the pull request. An AI-generated pull request must clearly say so in its
description.

## Review

At least one other developer must approve a pull request. Reviewers check:

- behavior and regression risk;
- callback and object lifetimes;
- storage and upstream compatibility;
- privacy and network impact;
- localization and interface scaling;
- platform-specific behavior;
- focused commits and sufficient verification.

Resolve all blocking comments before merge. Use squash merge for a noisy work
history; otherwise preserve clean, meaningful commits.

## Baseline verification

Before NexFort feature work starts, both developers should initialize
submodules, complete the documented platform-specific Debug build, launch the
client, and record the platform and result in the baseline pull request. Do not
use a Release build for this check.
