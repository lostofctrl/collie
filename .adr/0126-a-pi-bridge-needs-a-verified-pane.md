# 0126: A Pi bridge needs a verified pane

- **Status:** Proposed — direction confirmed; pane identity and supported Pi APIs need verification
- **Date:** 2026-10-30
- **Trail:** [ADR 0098](./0098-pi-questions-are-read-by-shape-and-footer.md) (screen-based Pi questions and the limits of answering custom extension UIs) · [ADR 0024](./0024-a-beacon-is-a-hint-never-a-control-channel.md) (an unverified emitter cannot authorize an action) · herdr-web's `pi-bridge` (a live Pi extension with a per-pane socket)

## Context

Collie mirrors existing Pi terminal sessions. Pi's standard RPC mode starts a separate process; it
is not an attachment to an existing terminal session, and its `ctx.ui.custom()` is unavailable.
A screen reader can safely lift some dialogs, but not every custom interface exposes enough
information for that. The herdr-web Pi bridge demonstrates another route: an extension in the live
Pi process can publish model state and relay RPIV questions while the terminal remains in use.
A local socket alone does not prove that its other end is the Pi process for the pane Collie shows.

## Decision

Pursue a Collie-owned, bundled Pi extension for the existing live session, initially for Herdr. Install
it in the user's global Pi setup by default, with an owner-controlled opt-out. A missing or disabled
extension must not prevent ordinary Collie use.

Structured answers and model changes require a verifiable binding to the intended live Pi pane.
If that binding cannot be established, do not perform structured actions: retain the existing
verified screen-based controls where possible and otherwise offer the terminal and keys. A question
answered from the terminal or a paired device closes for the others; a later answer is stale.
The extension is not a reason to relax the device, send, or prompt safety gates.

The target is broad safe coverage, including full RPIV question interactions and searchable model
selection, not a claim that every arbitrary custom terminal UI is machine-readable. Scope-list
editing needs a supported Pi API or a verified UI path; if neither is available, explain the limit.

## Consequences

- Stock RPC-only sessions and trusting an unverified same-user socket do not meet this direction.
- Prove pane identity and test the trust boundary before enabling structured actions; this record
  does not assert that a suitable mechanism has already been found.
- Check compatibility with existing Pi extensions, installation and removal, and the Herdr pane
  lifecycle. Plugin support for tmux and zellij is not part of the first integration.
- ADR 0098's screen-based rules remain the fallback. Any later change to an accepted ADR must follow
  the repository's amendment or supersession rules; this proposal does not silently change it.
