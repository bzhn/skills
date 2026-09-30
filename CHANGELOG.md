# Changelog

All notable changes to this plugin are documented here. Versions follow [Semantic Versioning](https://semver.org): major for renamed or removed skills and breaking output format changes, minor for new skills or new behavior, patch for wording fixes.

## 0.3.0 — unreleased

### Changed

- `review-to-gitlab-format`: finds the branch's merge request with glab or a GitLab MCP tool and links it in the note's `mr` property, like `"[myapp!123](https://…)"`. The chat reply shows the MR too.

## 0.2.0 — 2026-09-30

### Added

- `teach-by-asking`: teaches a topic by asking the user questions first and correcting their answers, with a map of the main parts, defined terms, a hint ladder, confidence checks, adaptive difficulty and a recap to review later. Its README explains the learning research behind it.

## 0.1.0 — 2026-09-29

### Added

- `review-to-gitlab-format`: converts a code review into paste-ready GitLab MR comments, saved as an Obsidian-friendly note in `~/.gitlab-review-notes/` with properties, an index table and "Posted" checkboxes.
