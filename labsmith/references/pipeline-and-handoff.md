# Content and media pipeline that survives a change of agent

Goal: any new agent, session or bot can continue writing articles, generating scripts, rendering
and publishing videos, attaching them to articles and auditing — without the previous
conversation. Achieve it with executable state, not with memory.

## 1. Four pieces

1. **Runbook** (one document, in the project's language): roles, the fixed start-of-session
   steps, one flow per kind of work (new article; scene or script change; video; review and
   audit), done criteria per flow, hand-off rules and a never-do list. Use
   `templates/pipeline-runbook.md`. Link it first in the agent instructions.
2. **Doctor** (`doctor.sh`): checks every prerequisite and prints OK / MISSING with the fix —
   runtime versions, dependencies installed, encoder, headless browser, storage credentials
   present (names only, never values), a signed request to the bucket, the public media domain
   reachable, disk space. A role flag skips media checks for content-only agents.
3. **Status board** (`status.py`): one row per article — scenes attached, script, video
   (ok / missing / stale), audit (date / never / changed) — plus a to-do list per role. It
   **fails** on errors (missing scene or script, stale video, missing captions, a standard named
   without sources) and lists to-dos without failing.
4. **Batch tools** checked into the repository: render + publish + register one film at a time
   (resumable: publish each as it completes), driven by the board's media to-do.

## 2. Make staleness detectable

- Store the script's content hash and version with each published video; the page generator and
  the board fail when the script changed without a new version, or the version differs.
- A script generator bumps the version of any script whose content changed.
- Record an audit as `{article: {date, content hash, log}}` (`--mark-audited`); the board shows
  `changed` as soon as the article's source differs from the audited hash.

## 3. Hand-off rules

- Git is the only hand-off surface: pull at start, commit and push at the end of every round,
  with the board's remaining to-dos in the commit message.
- Two bots never edit the same files: the content bot owns article sources, scenes, scripts and
  locales; the media bot owns the media manifest and generated pages.
- Interrupted work is safe: what was committed is the hand-off; the board lists the rest.
- Nothing is left as "remember next time" in chat.

## 4. Verify the pipeline itself

Mutation-test the guards once: edit a published script without bumping its version (board and
page generator must fail), edit an audited article (board must list it), unset a credential
(doctor must report it). Keep an end-to-end smoke check of the built site (lab boots, scene link,
watch / follow, unknown ids refused, article video and captions) and run it before releases.
