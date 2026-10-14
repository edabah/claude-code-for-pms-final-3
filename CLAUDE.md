# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch** (took over from Priya, who left 21 Aug 2026).
Sources: rook-wiki (Company section), `00-rook/company/notes/handoff-from-priya.docx`.
Wiki facts are as of the dates shown; check dates before trusting anything.

### Company and products
- Rook Industries sells coordination/provisioning software to masked, independent
  **responders** and their **handlers** and **quartermasters**. Rook employs no responders.
  Revenue is per active responder seat. HQ Site Aleph; offices in Berlin, Singapore, Cornwall.
- **Rook Dispatch** (mine): incident arrives in the handler web console, Dispatch ranks
  available responders (routing priority), pings the top one's phone, they take or
  turn down; a turned-down or missed ping moves to the next. Native phone app for responders.
- **Rook Supply**: requisitions -> quartermaster approval -> fulfilment -> maintenance schedule;
  field failure reports feed back. **Coupling:** Supply reads the Responder Availability Record
  (written by Dispatch) and books maintenance into the lowest-callout-load windows, so
  Dispatch routing changes silently shift Supply scheduling.
- Monthly release train, 4.x numbering. Routing config ships with a release, not a runtime setting.
- **Hard rule:** responder cover identities are never stored or mapped to legal identity
  (Security Policy 4.1). Never design for it; never try to work out who anyone is.

### Vocabulary
- **Callout** = request to attend an incident. **Ping** = a callout offered to one responder.
  **Taken / turned down / missed** (missed = ping wait expired; tracked separately from turned down).
- **Ping wait** = how long a ping sits before counting as missed (90s -> 60s in 4.2).
- **Routing priority** = score ranking responders: proximity (travel-time), availability,
  capability match, recent acceptance history. Declining or missing lowers recent acceptance,
  so lowers later rank.
- **Acceptance rate** (headline metric: taken / all pings), **time-to-accept** (median s),
  **coverage gap** (nobody with the needed capability tag; not the same as low acceptance).
- Capability tags: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic,
  crowd-management, de-escalation. **Mutual aid** = cross-area cover, unsupported, Q4 exploration.
- Supply: requisition, field failure report, service interval.

### People (Team directory, updated 2 Sep 2026)
| Person | Role | Notes |
|---|---|---|
| Helen Achebe | Director of Product (my boss) | Owns roadmap and commitments |
| Marcus Oyelaran | Eng Manager, Dispatch | Site Aleph; Priya's "start here"; wrote the 14 Aug config question |
| Wen Li | Staff Engineer, Dispatch | Berlin; built the pinging/ranking logic; only real source for how ranking works |
| Sofia Marino | Product Designer | Console and phone app; ran the September interviews |
| Ravi Menon | Data Analyst | Singapore; owns the weekly pick-up-rate numbers |
| Nadia Hoffmann | Support Lead | Berlin; owns tickets; standing 15 mins recommended |
| Priya Raghunathan | Former PM | Gone; no contact route except via Marcus |

**Role-to-name map for wiki comments** (4.2 release page uses roles only):
Engineering manager = Marcus; Staff engineer = Wen; Product designer = Sofia;
Support lead = Nadia; Priya = Priya. Handlers named in the interview/handler pages
(Aunt Dot, Mr. Ambrose, Halloran, Kip) are customers, not staff.

### Where things stand
**Releases:** 4.0 (7 Apr: new nav, profile redesign, routing-override audit log), 4.1 (16 Jun:
travel-time proximity, bulk callout, push reliability), 4.2 (12 Aug: proximity weighted up
vs recent acceptance, ping wait 90 -> 60s, persistent console filters, 3 defect fixes).

**4.2 is the live problem.** Since 12 Aug: callout tickets ~3x normal, steady not improving
(26 Aug), split about 2/3 "phone never goes off" and 1/3 "gone before I could answer".
A handler emailed Nadia directly, which is unusual.
- The 1/3 theme is plausibly the 60s ping wait. Nobody has explained the 2/3 theme.
- Priya's and Marcus's view: seasonal August dip, will recover in September. **That is an
  unverified hypothesis**, never checked against data, and two changes shipped together,
  so cause is confounded. Priya also warned against framing this as "revert 4.2".
- Marcus asked (14 Aug) whether the proximity change was meant to apply to responders who
  have been declining; the config doesn't distinguish. Wen was away 14-24 Aug and never
  answered. Open. Possible mechanism to test, not a conclusion: the shorter ping wait
  means more misses, and misses lower recent-acceptance, which lowers rank for later
  callouts (a feedback loop). Proximity weighting up should make acceptance history matter
  less, so don't assume it compounds the loop.
- Nobody has pulled Ravi's weekly acceptance numbers; Nadia asked for the trend 18 Aug and
  only a "rough" engineering pull was offered. Priya's note said to ask Marcus for numbers, but
  Ravi owns them. Marcus and Nadia planned a regroup with me after my first week; not confirmed held.
- Nobody has written down how ping order is decided (Priya asked me to). Source: Wen.

**Roadmap (Q3, last reviewed 30 Jun, stale):** all items owned by Priya. Change to who gets
pinged, Availability Confidence, Ping timeout tuning: Committed for 4.2. Requisition approval
chains: Committed 4.3. Handler phone app, Shared cover between responders: Exploring, Q4.
- Availability Confidence is not in the 4.2 release notes; it was probably squeezed out
  (Priya hinted at items dropped). Need Helen to confirm which Q3 commitments still stand;
  that conversation hasn't happened. Ping timeout tuning shipped only as a cut to 60s.
- Committed items are "locked"; changes go through Product (Helen).

**Product briefs (wiki > Product briefs) are partly stale:**
- Routing Override Audit Log (Mar): shipped in 4.0, so brief is out of date.
- Bulk Callout (Jan): shipped in 4.1, but brief says unowned and has no baseline metric.
- Handler Phone App (8 Sep): handler push/status/one-tap contact; Sofia exploring for Q4,
  no build staff. Motivated by Aunt Dot's "not just him" request. Distinct from the existing
  responder phone app.
- Requisition Approval Chains (Jul, Supply, 4.3): ask is a second approval over $2,000, but
  the brief bolts on routing-around, a cross-site dashboard, handler visibility and replacing
  Halloran's spreadsheet. Scope creep; needs trimming before it is committed.

### Working notes
- Priya's other advice: ignore filter-persistence tickets; use a fresh outsider's eye in month one.
- Treat Priya's handoff reading of 4.2 and the wiki's committed statuses as claims to verify.
- Rook is fictional; nothing here is a real-world fact.

### Added at end of Module 1 session
- Most likely cause of "phone never goes off" to test first: the 60s ping wait causes more misses, misses lower recent-acceptance, and lower rank means fewer pings (a loop). Unproven; proximity weighting up should reduce, not add to, that effect.
- Fastest checks, not yet run: weekly acceptance by responder (taken / turned down / missed) before and after 12 Aug and vs last August; override counts in the audit log; handler-calls-directly tickets. The rook-database connector is available for this; Ravi Menon has the weekly numbers.
- Customer interviews database (Sofia, September) is still unread and is the likely source of first-hand accounts.
- Open with people: Wen Li on how ranking works and Marcus's 14 Aug question; Helen on which Q3 commitments still stand (Availability Confidence missing from 4.2) and who owns Supply product; Nadia's ticket breakdown.
- Wiki is stale in places: Audit Log (4.0) and Bulk Callout (4.1) briefs describe shipped work; roadmap last reviewed 30 Jun.

### Added at end of Module 2 session
- Sources read: Customer interviews (4 handlers, 2-5 Sep: Aunt Dot, Ambrose, Halloran, Kip) and the support_tickets table (147 rows, 29 Jun-7 Sep). Overlap is almost nil: only Ambrose (Captain Vantage) is in both. Ticket data ends 7 Sep, so newer tickets may exist.
- Tickets: "gone before I could answer" = 15 tickets from 11 of 12 handlers (none before 12 Aug); "phone gone quiet" = 30 tickets but only 4 responders (Farlight, The Undertow, Halfmoon, Corporal Ashgrove), repeated follow-ups, all open. Farlight/Undertow went from ~12 pings/week to 1-3. Three responders have both problems. Nobody files tickets about being flooded (Kip's The Gale shows it only in interviews).
- Interviews vs tickets: interviews loud on alerts, text size, dark mode (rare in tickets); tickets show Supply (26) and ~46 housekeeping tickets (many identical-text pairs) nobody raised. Halloran praises maintenance scheduling but tickets 3054/3109 booked servicing on the marathon day. Ambrose's ticket 3043 dates his incident 12 Aug; his interview says "two weeks ago".
- Open question: are quiet phones really not being offered pings, or offered and missed unnoticed? Needs per-responder weekly offered/taken/turned down/missed. I have not yet listed rook-database tables beyond support_tickets to find a pings table. No interviewee mentioned proximity.
- Next steps: Ravi's numbers (four quiet responders plus controls incl. The Gale), Wen on ranking and Marcus's 14 Aug question, Nadia to send holding replies to open tickets, Helen on Q3 commitments. If misses rose, consider changing only the ping wait in the next release rather than "revert 4.2".

- rook-database has 5 tables: pings (callout_number, responder, sent_at, outcome; 1,696 rows, 29 Jun-6 Sep), callouts, responders (name, handler, area; one handler per responder, Kip has two), handlers, support_tickets. Pings answers the open question: missed pings jumped on 12 Aug itself, from 25 of 1,085 pings (2.3%) before to 110 of 611 (18%) after, easing weekly to 12.7%. Turned down stayed flat (~21% to 18%). Pings per callout ~1.25 to ~1.3; callouts per day fell ~20 to ~17. Pings and tickets were both flat before 12 Aug, so a seasonal dip is not supported.
- Four responders collapsed: Farlight (Linda Pruitt), The Undertow (Desmond Okafor), Meteor Mite (Kip), Vesper (Aunt Dot). Their weekly pings went ~49 to 43, 16, 6, 3 and they now miss over half; the other 12 absorbed it (+21%). Handler and area don't explain it: Kip's Meteor Mite fell 66% while his The Gale rose 42%.
- Fewer pings = lower rank (pings go down a ranked list until someone takes it). Farlight was pinged for all 59 Uptown callouts before 12 Aug (first for 44) and 6 of 31 after (first for 4); Falkirk and Cindermark became first choice. Fits the miss-then-rank-drop loop but I can't see ranking scores, so Wen has to confirm. Proximity alone doesn't obviously explain it.
- Tickets vs pings agree on timing (first ticket 12 Aug 15:41, The Drift) and Farlight/Undertow "days since a ping" claims match the pings table exactly. They disagree on who: Meteor Mite and Vesper have zero tickets (Kip, Aunt Dot filed none), while Halfmoon and Corporal Ashgrove are loud in tickets but only ~20% down in pings. Quiet tickets start 17 Aug and peak the week of 24 Aug; "gone before" tickets peak in release week then fade although misses continue.
- Still open: why those four (tables hold no location, capability or ranking data); why callouts fell; unfilled callouts rose 5.5% to 11.1% (Southport 3 to 9, Riverside 2 to 6, small numbers, unchecked). Next: call Aunt Dot and Kip, reply to Linda's and Desmond's open tickets, get Ravi to confirm these numbers and Wen to explain the ranking.

- Module 4 (code read of 00-rook/code/dispatch-routing, 7 files): 4.2 changed only config.py: ping wait 90 to 60s, travel weight 0.45 to 0.60, acceptance weight 0.40 to 0.25 (skills 0.15). The old values come only from code comments and the changelog (git history is one commit). A taken ping adds 0.08, a turned-down or missed one takes off 0.12 ("same either way", history.py); nothing fades scores (2019 TODO), and in this copy they sit in an in-memory dictionary. Phone and availability functions are empty stubs.
- My replay of the pings table (an estimate, not the real scores): a score only drifts down if a responder fails to say yes on more than 40% of pings. Before 4.2 nearly everyone sat at 0.85-1.0, so history barely ranked anyone. The four collapsed responders miss 53-64% since 4.2 (others 10-18%) and hit 0 by 6 Sep. Past turn-down rate did not predict who collapsed.
- Proximity is not supported as the cause: each city area has one home responder, and the home responder was first pinged 80% of the time before 4.2 and 61% after, with the drop concentrated in Uptown, Harborside and Old Town. A heavier travel weight should raise that. By the formula a sunk score costs at most 0.25 points, so the loop alone looks too weak; still unexplained why those four. Candidates for Wen: skills match, the travel-time estimate, availability settings, or deployed code differing from this copy.
- Marcus's 14 Aug wiki comment asks whether the proximity change was meant for responders who'd been turning jobs down ("a decision or just fell out that way"). The code treats everyone equally. He wrote "went clean" on 13 Aug and nobody had raised a problem yet, so it reads as an intent question, not debugging. The release page says the proximity change was "long requested by responders who cover wide areas". Nobody has answered (Wen away to 24 Aug, Priya gone), so Helen or Wen must confirm intent.
- Supply reads availability.py's record (its shape is a contract). Whether the changed ping pattern moved Supply servicing is unchecked.
- Questions to take to Wen: are scores saved across deployments or reset at each release; should a miss cost the same as a turn-down; should scores fade; what are the four responders' travel times and skills for their own area's callouts.
