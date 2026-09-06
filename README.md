# Flutter Scalable BLoC Skill

Reusable agent guidance for building maintainable Flutter apps with a feature-oriented layered architecture, Cubit/BLoC, repository contracts, dependency injection, network boundaries, and platform services.

It is derived from production patterns, but contains no application-specific business logic, credentials, endpoints, or client assets.

## What it helps with

- Starting or refactoring a multi-feature Flutter app
- Keeping UI, business rules, transport, persistence, and device integrations separated
- Building thin feature modules with Cubits and explicit state
- Avoiding duplicated widgets, endpoint strings, DTO leakage, and platform checks
- Implementing cursor pagination and deliberately scoped offline behavior
- Producing compact responsive UI that respects native Android/iOS conventions

## Install

### Codex

```bash
npx skills add <YOUR_GITHUB_ORG>/flutter-scalable-bloc-skill --skill flutter-scalable-bloc
```

Or clone the repository and place/symlink `flutter-scalable-bloc/` in `~/.codex/skills/`.

### Claude Code

Clone the repository, then place or symlink `flutter-scalable-bloc/` in `~/.claude/skills/`.

```bash
ln -s /absolute/path/to/flutter-scalable-bloc-skill/flutter-scalable-bloc ~/.claude/skills/flutter-scalable-bloc
```

The core is a portable `SKILL.md` plus Markdown references, so it is designed to work in both hosts.

## Use

Invoke it explicitly when beginning a Flutter feature or refactor:

```text
$flutter-scalable-bloc Add an offline-aware orders screen to this Flutter app.
```

Also state the product scope. For example:

```text
$flutter-scalable-bloc
Implement a buyer order-history screen. Preserve the current architecture and dependencies.
Use cursor pagination, keep old results visible during pull-to-refresh, and do not add offline writes.
```

## Benefits

- Less duplicate code and fewer premature abstractions
- Clear boundaries that make features safer to change
- Faster agent orientation in structured Flutter projects
- More consistent UI and platform behavior
- Architecture guidance loaded progressively instead of a large README on every task

## Before publishing

1. Replace `<YOUR_GITHUB_ORG>` in this README.
2. Test the skill on a small app, a new feature, and a refactor.
3. Keep examples generic; never commit client code, tokens, URLs, or private documents.
4. Add a license—MIT is a practical default if you want broad reuse.
