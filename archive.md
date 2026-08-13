# NexFort — Archived

> **Development of NexFort has been discontinued.**

NexFort was an experimental Telegram client based on AyuGram.

The project started as an attempt to build a Telegram client with our own
features, behavior and UI changes while keeping compatibility with the
Telegram ecosystem.

## Why is NexFort archived?

Maintaining a fork of a large and actively developed Telegram client turned
out to require significantly more maintenance than expected.

Every upstream Telegram/AyuGram update could introduce changes that required
NexFort's modifications to be reviewed, adapted, merged and tested again.

Over time, maintaining compatibility started taking more effort than actually
building the features that NexFort was created for.

Since the project is still in an early stage, we decided to stop development
instead of turning it into a permanent upstream-maintenance project.

## Project status

NexFort is no longer actively developed.

The repository is preserved as an archive of the work, experiments and ideas
created during development.

The existing code may still be useful for reference or for future projects,
but no compatibility with future Telegram or AyuGram versions is guaranteed.

## What we learned

NexFort was not wasted work.

The project provided experience with:

- working with a large existing codebase;
- maintaining and modifying an upstream project;
- debugging and auditing code;
- Git workflows and resolving integration problems;
- designing features around an existing application architecture;
- understanding the long-term maintenance cost of software forks.

One of the most important conclusions from NexFort was simple:

**Writing a feature once and maintaining that feature indefinitely are two
very different engineering problems.**

## Future

There are currently no plans to continue NexFort.

Some ideas, code and lessons from the project may eventually appear in other
projects with architectures that are easier to maintain.

Thanks to everyone who followed or contributed to the project.

— NexFort
