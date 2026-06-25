# Searching Logs with KQL

The companion to the video. Open Kibana in one tab and this file in the other. Read down the cheat table, then run the exercise at the bottom against the course dataset (the `soc-lab` data view you loaded during setup).

## 📺 Watch the Video

🎥 [Coming soon — link will be added once the episode publishes]

## 🎯 What You'll Walk Away With

- Knowing the difference between free text search and field search, and why you live in field search
- Knowing the handful of operators that cover almost every query you'll write
- Being able to combine a query with the time range and filter pills
- Being able to answer a real investigation question from a blank search bar

---

## Quick Reference

The whole grammar on one screen. Bookmark this.

| Pattern | Example | What it does |
|---------|---------|--------------|
| `field:value` | `process.name:sshd` | Match a field exactly |
| `"quoted value"` | `message:"Failed password"` | Match a value that has spaces |
| `AND` | `process.name:sshd AND event.dataset:system.auth` | Both must be true |
| `OR` | `process.name:sshd OR process.name:sudo` | Either is fine |
| `NOT` | `NOT process.name:CRON` | Exclude it (values are case-exact, cron logs as `CRON`) |
| `( )` grouping | `process.name:sshd AND (host.name:web-prod-01 OR host.name:web-prod-02)` | Control evaluation order |
| `field:(a OR b)` | `process.name:(sshd OR sudo OR CRON)` | One field, several values |
| `*` wildcard | `process.name:ssh*` | Match a family of values |
| `> >= < <=` | `process.pid > 1000` | Ranges on numbers and dates |
| `field:*` (exists) | `user.name:*` | Field is present at all |

Operators go in UPPERCASE. No spaces around the colon.

---

## Lesson Notes

### Free text vs field search

Type a bare word (`sshd`) and Kibana matches it anywhere in the document. Fast, blunt, good for a first look. Type `process.name:sshd` and it only matches where that field is exactly `sshd`. Precise. You investigate in field search.

### The grammar

Every field search is `field : value`, no spaces around the colon. Everything else is a variation on that:

- Wrap values with spaces in double quotes, or the search breaks.
- `AND`, `OR`, `NOT` combine conditions.
- Parentheses control order the moment you mix `AND` and `OR`. When in doubt, parenthesize.
- `field:(a OR b)` matches one field against several values without repeating the field.
- `*` is a wildcard. Avoid leading wildcards (`*ssh`), they're slow.
- `>`, `>=`, `<`, `<=` work on numbers and dates. Let the time picker handle most date bounding.
- `field:*` asks whether a field exists at all, regardless of its value.
- Values are case-exact. `process.name:cron` finds nothing because the value is `CRON`. Match what you see when you expand a document.

### The pivot

The single most important move in the worked example: click a value in a result row and Kibana adds it as a filter pill. That turns one suspicious line into everything around it. Find a thread, pull it.

---

## 🧪 Exercise: Write the Queries

Open Discover, select the `soc-lab` data view, and set the time picker to an absolute range that covers the data:

```
June 7, 2026 00:00  →  June 12, 2026 00:00
```

Now write the KQL for each question. Everyone searches the same dataset, so there are real answers, and your counts should match Check Your Work below. The window above is deliberately wide so it catches all the data whatever timezone your Kibana shows. If a query comes back empty, it's almost always the time range or a leftover filter pill, not your syntax.

- [ ] Question 1: Show only SSH activity. (Hint: `process.name`)
- [ ] Question 2: Show SSH activity that was NOT a successful login. (Hint: `NOT` + `event.outcome`)
- [ ] Question 3: Show events from either `sshd` or `sudo` in a single query, without repeating the field name. (Hint: `field:(a OR b)`)
- [ ] Question 4: Show every event that has a `user.name` populated. (Hint: the exists pattern)
- [ ] Question 5: Pick one suspicious value from your results, click it to add a filter pill, and write down what else that IP or host did.

### Check Your Work

<details>
<summary>Click to reveal the counts (try the queries first)</summary>

- Question 1: **150** documents
- Question 2: **53** documents (45 are the attack, 8 are honest internal fat-fingers, which is why you need more than `event.outcome:failure` to isolate the real problem)
- Question 3: **192** documents
- Question 4: **192** documents
- Question 5: one external IP, `203.0.113.45`, is behind 45 failed SSH logins and then **one** `Accepted password` login for the `test` account, followed by a `sudo` to root. Pivoting on that IP shows all **46** of its events. That's the compromise.

If your numbers are off, check the time range first, then look for a stray filter pill.

</details>

### Share

Post in the [Apprentice Discord](https://discord.gg/XjHJFp4KSm) thread for this episode with the query you wrote for Question 2. Everyone's on the same data, so you should all land on the same count. If yours is different, say so and we'll work out why.

---

## 🔑 Key Takeaways

- **Field search beats free text.** `process.name:sshd` is precise, a bare `sshd` is a guess.
- **Quote values with spaces** or the query silently breaks.
- **`AND` `OR` `NOT` plus parentheses** cover almost everything. Parenthesize when you mix them.
- **`field:*` checks existence**, not value. Underused and powerful.
- **The pivot is the job.** Click a value, follow it, build the picture.
- **Time range first.** A query that finds nothing is usually a time picker problem, not a syntax problem.

---

## ➡️ What's Next

You can now ask the data any question you want. The next step is keeping the questions you'll ask again, so you're not retyping them every shift, and turning them into a view you can watch at a glance.

[← Back to Week 1 Overview](./README.md) | [Next →](./06-saved-searches-and-dashboards.md)
