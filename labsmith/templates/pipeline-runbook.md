# <NN> Content and media pipeline runbook (for agents and bots)

Version <x> (<date>). Any agent, session or bot works from this document; it does not rely on
earlier conversations. Details live in the authoritative documents below; this runbook gives
order, commands, done criteria and hand-off.

| Authoritative document | Covers |
| --- | --- |
| <authoring guide> | topics, structure, terms, sources, lab sync |
| <review guide> / <audit guide> | periodic review / statement audit |
| <reenactment spec> | script format, shots, rendering, media storage |
| <agent instructions> | project rules and the pre-commit gate |

## 1. Roles
| Role | Does | Does not |
| --- | --- | --- |
| Content bot | articles, scene links, scripts, review, audit, mark audited | render, upload |
| Media bot | doctor, batch render, upload, register, regenerate pages | edit articles or scripts |

## 2. Every session
1. Pull; read the agent instructions; state which companion skills are available.
2. `<doctor command>` (`--content` for the content bot); stop and report any MISSING you cannot fix.
3. `<status command>`; fix ERRORs first; take only your role's to-dos; re-run at the end.

## 3. Flow A — new or changed article (content bot)
plan → write all languages → sources for any named standard → attach scenes → `<generate scripts>`
→ tests + page generator → audit and `<mark audited>` → gate → commit and push.
Done: board row has scenes, a script, today's audit date, no ERROR.

## 4. Flow B — scene or script change (content bot)
regenerate scripts (versions bump automatically) or edit a hand-written script and bump its version
→ tests → sparse draft render and look at every frame → map the article → commit.

## 5. Flow C — videos (media bot)
build → `<batch render>` (resumable) → commit every one or two films with the remaining count.
Done: media to-do is none, page generator passes, a live article plays and downloads.

## 6. Flow D — review and audit (content bot)
review / audit logs by date; fix severe and important findings at once; mark audited.

## 7. Hand-off
Board free of errors, gate green, committed and pushed, remaining to-dos in the commit message.

## 8. Never
print or commit secrets; overwrite published media; skip or delete checks; hand-edit generated
files; accept an invalid measurement; add dependencies or change security headers without review.
