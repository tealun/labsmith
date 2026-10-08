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
   **fails** on errors (missing scene or script, a script edited without a new version, missing
   captions, a standard named without sources) and lists to-dos — missing or stale videos, clips,
   audits — without failing.
4. **Batch tools** checked into the repository: render + publish + register one film at a time
   (resumable: publish each as it completes), driven by the board's media to-do.

## 2. Make staleness detectable, and harmless

- Store the script's content hash and version with each published video. A script edited
  without a new version is an **error** (board and page generator fail). A video from an older
  version is **stale**: pages hide it automatically and the board lists it as media to-do, so the
  content side is never blocked waiting for a render.
- Upload a release marker (`release.json`: version, script hash, files, titles) **last**. A version
  is published once its marker exists; objects without a marker belong to an interrupted upload
  and may be replaced. If a session dies after uploading but before committing, the next run
  *adopts* the marked release instead of rendering again.
- The batch tool builds the site itself and serves it on its own port, and the render harness
  refuses a build whose bundled script version differs from the file — a stale build can never be
  filmed under a new version.
- Track short clips the same way: record which preset each clip shows; a changed preset puts the
  clip on the to-do list.
- A script generator bumps the version of any script whose content changed.
- Record an audit as `{article: {date, content hash, scenes hash, log}}` (`--mark-audited`); the
  board shows `changed` as soon as the article's text — or the labels of the scenes it links —
  differ from what was audited.
- Never rename or delete a published preset id (it is in links, media paths and clip manifests);
  add a new id when the meaning changes.

## 3. Hand-off rules

- Git is the only hand-off surface: pull at start with an explicit strategy (`git pull --no-rebase`
  avoids flipped ours/theirs), regenerate after every pull that brought the other bot's commits,
  commit and push at the end of every round with the board's remaining to-dos in the message.
- List which files are generated and which are hand-written; only generated files may be resolved
  by taking one side and regenerating. Source conflicts are merged edit by edit, or reported.
- Give the agent an exit: when the docs do not allow a request (a page about a capability the lab
  does not have), it changes nothing and reports why, citing the section.
- Two bots never edit the same source files: the content bot owns article sources, scenes,
  scripts and locales; the media bot owns the media manifest and media files. Generated pages are
  rewritten by both: on a merge conflict take either side and regenerate, never merge by hand.
- Interrupted work is safe: what was committed is the hand-off; the board lists the rest.
- Nothing is left as "remember next time" in chat.

## 4. Verify the pipeline itself

Before trusting it, have a fresh agent with no context follow only the runbook in a dry run
(no edits, no uploads) and list every ambiguity, contradiction or failing command; fix them and
version the runbook. The first such run on the reference project found 15 gaps.


Mutation-test the guards once: edit a published script without bumping its version (board and
page generator must fail), edit an audited article (board must list it), unset a credential
(doctor must report it). Keep an end-to-end smoke check of the built site (lab boots, scene link,
watch / follow, unknown ids refused, article video and captions) and run it before releases.
