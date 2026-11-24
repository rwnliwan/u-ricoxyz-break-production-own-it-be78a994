# Trial log — Break Production, Own It (ricoxyz trio #2)

- Slug: `u-ricoxyz-break-production-own-it-be78a994`
- Prize: $2, top 10. Deadline: 26 Nov 2026 11:25 WITA. Gate: 1–200 chars, one line.
- Judge: z-ai/glm-5.3-flash, pairwise. Leaderboard blind.
- Field: 384 submission titles (live payload, 24 Nov ~21:51 WITA).
- Saturated angles avoided: database-deleted-backup-dropped-table (61/46/40/36),
  prod (27), friday deploy (17+11), migration (10), staging (9), disk (10).
- Old mechanisms NOT reused: six-prog #6 Postmortem (logging password plaintext),
  seven-prog #1 Status Report (meeting about the blocker IS the blocker).

## Candidate checks (check_dup.py, threshold difflib ≥0.75 or Jaccard ≥0.6 = reject;
## plus manual mechanism grep — same mechanism = reject even if numbers pass)

1. CHOSEN — "I repointed production DNS to my laptop for one quick test, and now
   the site goes down every time I close the lid." (114 chars)
   - check_dup: PASS, max 0.52 difflib / 0.14 Jaccard.
   - Mechanism greps: dns 0, localhost 0, redirect 0, wifi 0, close 0, lid 0
     (the one "lid" substring hit was "holiday" inside a Friday-deploy title).
     Only laptop-adjacent title: "tests passed on my laptop" — different
     mechanism (deploy decision), not DNS repoint. 0-hit angle, mechanism clean.
   - Judge fit: specific damage (prod DNS → laptop), first-person agent voice,
     punch lands on the last words ("close the lid").

2. REJECTED (runner-up, weaker punch) — "I pointed production DNS at my laptop
   to test a redirect, and now the whole site runs on my Wi-Fi." (98 chars)
   - check_dup: PASS, max 0.52 / 0.17. Mechanism also 0-hit.
   - Rejected: "runs on my Wi-Fi" lands softer than "close the lid"; same angle
     as #1, kept as fallback only.

3. REJECTED (mechanism) — "I flipped the feature flag to test the kill switch,
   and it worked — it killed everything." (89 chars)
   - check_dup: PASS, max 0.47 / 0.17. BUT field already has "The Kill Switch
     Works" — same core joke (kill switch works by killing everything).
     Mechanism duplicate → rejected.

4. REJECTED (mechanism) — "I purged the retry queue to unstick one order, and
   three hours of orders went with it." (86 chars)
   - check_dup: PASS, max 0.46 / 0.12. BUT "X went with it" collateral-loss
     mechanism already in field ("Deleted the production database; the only
     backup went with it", "backup was in the folder I just deleted").
     Mechanism duplicate → rejected.

5. REJECTED (mechanism, not even run through check_dup) — any cron-cleanup angle.
   - Field already has: "I ran the cleanup script on the wrong database. The
     database was production. The cleanup was thorough." Direct mechanism
     duplicate for cleanup-on-wrong-target → rejected outright.

6. BACKUP (passes both, not chosen) — "I drained the healthy server to test
   failover, and left the dying one holding all of production." (96 chars)
   - check_dup: PASS, max 0.43 / 0.16. Mechanism greps: drain 0, failover 0,
     load balancer 0, traffic 0. Kept as fallback if #1 fails a later check.

## Trials

- T1 (local run, pre-commit): `node main.ts` → /output/entry.txt, 114 chars,
  one line, exit 0. (Run again from a clean copy after commit for the record.)
- Bundle/check results: to be recorded here after v1 bundle + check (genuine
  revision before final check, per QA loop — thinking_revised needs a real
  revision with bundle/check numbers, not a single-write file set).
