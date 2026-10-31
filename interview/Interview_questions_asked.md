## Real interview: YourDOST (Product Manager), Puneet (co-founder), 2026-10-29

### Tell me about your family background / where you grew up [2026-10-29]
**A:** (keep to ~1 min, end with what it gave you that's relevant to the role)
**Status:** ✅

### Our workload is heavy (3 PMs for the company). How's your workload now? [2026-10-29]
**A:** At PCL I already run several workstreams at once: client reporting, migration, BI rollout.
Show you can handle a heavy load before you're asked about pressure.
**Status:** 🔶

### Why do you want to leave? [2026-10-29]
**A:** I want to own a product end to end, from user problem to metric, not split my time across
everything a startup needs. YourDOST is a product I've used myself, so I'd be building for a user I understand.
**Status:** 🔶 (said "it's getting all-functional", which sounds like a complaint)

### How do you handle pressure? [2026-10-29]
**A:** STAR story. E.g. Redwood migration wiped a client's 5+ years of historical reports: I set priorities,
wrote the requirements, and delivered a SQL-based replacement with zero data loss.
I plan ahead, and when pressure comes anyway, I break it into the next concrete step.
**Status:** 🔶 (said "I avoid pressure, get a little anxious" right after hearing the workload is heavy)

### Do you know our app/website? [2026-10-29]
**A:** Yes, I used it at IIT Guwahati during a hard transition. Next round: add what worked,
what didn't, and one thing I'd change as PM.
**Status:** 🔶 (product opinions needed)

### Are you better with numbers or creative work? [2026-10-29]
**A:** Both, with one example each: numbers = fraud threshold tuning (false declines −98.6%);
creative = repositioning a stalled AI product into native OTBI/Power BI reporting.
**Status:** 🔶

### How were you academically on campus? [2026-10-29]
**A:** Top 50% of ~200. Lead with what else I built: 4i Labs submarine team, Inter IIT table tennis
(1–2 h/day of coaching alongside classes).
**Status:** ✅

## Real interview: Prowess (Data Analyst, consulting-style), 2026-10-27

### How many rows does a LEFT / INNER / RIGHT join return? (A has 10 rows, B has 5) [2026-10-27]
**A:** LEFT JOIN keeps every row of A, fills B's columns where the key matches, else NULL.
Row count is at least 10, and more if one A key matches several B rows (max 10 × 5 = 50).
INNER keeps only matching pairs: 0 to 50 rows. RIGHT is the mirror of LEFT: at least 5.
FULL OUTER keeps everything from both (MySQL has no FULL JOIN: use LEFT UNION RIGHT).
Payments example: transactions LEFT JOIN merchants, so transactions with an unknown merchant still appear.
**Status:** 🔶 (said "10 rows" in the interview; missed duplicate keys)

### VLOOKUP syntax [2026-10-27]
**A:** =VLOOKUP(lookup_value, table_array, col_index_num, FALSE). Use FALSE for an exact match.
Limits: can't look left, defaults to an approximate match, breaks when columns are inserted.
INDEX/MATCH or XLOOKUP fixes all three.
**Status:** ✅

### Have you used pivot tables? [2026-10-27]
**A:** (to learn) A pivot table is a drag-and-drop GROUP BY: Rows = group key,
Values = SUM/COUNT, Filters = WHERE.
**Status:** 🔶 (answered "no", so practice Excel 2.3)

### Python: how hands-on are you? [2026-10-27]
**A:** I used pandas in a data migration: I read the client Excel files, mapped their columns
to Oracle Spreadsheet Loader's columns, and matched records automatically
(10,000+ records, 95% accuracy, then 100% in the final load).
Give the result, not just the library name.
**Status:** 🔶

### Power BI: dashboard or report? [2026-10-27]
**A:** A report is multi-page and interactive, built in Desktop on one dataset.
A dashboard is a single page in Power BI Service made of tiles pinned from one or more reports.
My HR analytics work (7 pages) is a report.
**Status:** 🔶

### Why are you looking to change? [2026-10-27]
**A:** (better version) At PCL I've worked across a wide range of problems, and the work I enjoyed most
was the data work: the SQL reporting rebuild, the migration pipeline, the Power BI rollout.
I want to go deep in analytics full-time, in a team where data drives the decisions.
**Status:** 🔶 (said "random work", which sounds negative)

## Real interview: Shine.com, Oct 16

### What's your favourite product, and what feature would you change? [2026-10-16]
**What I said:** Had no prepared answer; pivoted to Shine.com and discussed
its strengths and gaps.

**A (what I'd say now):**
- **Product:** LinkedIn. I use it daily for jobs and learning.
- **Pain point:** Applying feels like a black hole. After Easy Apply, I
  usually get no signal whether anyone saw my application.
- **Change:** An application status tracker (Submitted -> Viewed by
  recruiter -> Shortlisted / Closed), with an auto "position closed" update.
- **Why it matters:** Candidates who get feedback come back; silence makes
  them apply less carefully and churn.
- **How I'd measure it (analyst angle):** A/B test. Primary: 30-day
  applicant return rate. Secondary: applications per active user,
  recruiter response rate. Guardrail: recruiter workload / time-to-respond.
- **Tie back:** The same idea would work on a job portal like Shine.

**Status:** 🔶 (prepared, not yet said aloud)

## Real interview: [company], Oct 19

### How can we increase candidate engagement on the platform? [2026-10-19]
**A (what I'd say now):**
1. **Clarify "engagement":** weekly active candidates? applications? return
   visits? Pick one north-star metric (e.g. weekly active applicants) first.
2. **Segment users:** freshers vs experienced; active job seekers vs
   passive (employed, just browsing). They need different things.
3. **Find the drop-off in the funnel:** sign-up -> profile complete ->
   search -> apply -> recruiter response -> return visit. Fix the
   biggest leak first, not every stage.
4. **Ideas per stage:**
   - Profile: completion nudges ("complete profiles get X% more views").
   - Search/alerts: more relevant job alerts (fewer, better matches).
   - After applying: status updates (the "black hole" problem).
   - Passive users: salary insights, skill tests, career content.
5. **Measure:** A/B test each idea. Guardrails: notification opt-out
   rate, uninstalls (too many alerts kill engagement).
6. **SQL angle:** weekly cohort retention: group users by sign-up week,
   count how many are active in week 1, 2, 3... That shows whether a
   change actually brings people back.

**Status:** 🔶
