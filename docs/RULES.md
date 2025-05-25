# ── Dotfile Doctor – Agent Rules ───────────────────────────────────────────────
rules:
  # ── Safety & Version-Control ────────────────────────────────────────────────
  - Never push, force-push, or merge directly to the main branch.  
  - Every change must live in a feature branch prefixed `alias/` or `fix/`.
  - Commit messages follow Conventional Commits (`feat:`, `fix:`, `chore:` …).
  - If CI fails, abort the PR and notify the user; do not auto-retry.

  # ── Alias Generation ────────────────────────────────────────────────────────
  - All new aliases must be appended **only** to `zsh/20-aliases.zsh`.
  - Check for collisions: if an alias name already exists, prompt the user
    to *override*, *rename*, or *skip*; never auto-override.
  - Default flags added to an alias must be
      • idempotent, and  
      • easily overridden by the user’s own flags.  
    (Use long flags where clarity beats brevity.)

  # ── File & Directory Constraints ────────────────────────────────────────────
  - Do not create or touch files outside `$DOTFILES_REPO`.
  - Run `stow --restow` **only** after a successful commit in the current repo.
  - Never delete or rename existing files; append or insert text only.

  # ── User Interaction & Transparency ────────────────────────────────────────
  - Always present a diff in Cascade before committing.
  - Surface the `rationale` for each alias in plain English (≤ 120 chars).
  - Log every tool-call with a timestamp to `~/.local/share/dotdoctor/agent.log`.

  # ── Resource Use ───────────────────────────────────────────────────────────
  - Keep resident RAM below 200 MB; if memory spike > 300 MB, auto-restart.
  - Tail log files with non-blocking I/O; no busy-waiting allowed.

  # ── Secrets & Network ──────────────────────────────────────────────────────
  - Read GitHub tokens only from `$GITHUB_TOKEN` env var; never log them.
  - Uploads to external APIs (OpenAI, GitHub) must redact user PII in payloads.
  - If the network is unavailable, queue events in SQLite and retry with
    exponential back-off (max interval 15 min).

  # ── Undo & Recovery ────────────────────────────────────────────────────────
  - Store the last successful commit hash in SQLite (`state.last_good_commit`).
  - Provide an `undo` command that rolls back to `last_good_commit`
    and restores previous symlinks with `stow -D && stow`.

  # ── Extensibility Guards ───────────────────────────────────────────────────
  - All new tools must be declared in `mcp.yaml`; never spawn undeclared
    executables.
  - Any addition of a new package manager (Flatpak, Cargo, Brew) requires
    a separate watcher module and its own rules block.

# ── End of Rules ──────────────────────────────────────────────────────────────
