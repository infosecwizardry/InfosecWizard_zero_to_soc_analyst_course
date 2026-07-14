# Section 1: SIEM Fundamentals

A SIEM is where you'll spend most of your day as a SOC analyst. This section gets you fluent in it: what a SIEM is, how to read the logs inside it, and how to search, filter, and pivot through data without second-guessing where to click. Everything later in the course assumes you can do this.

## 📺 Watch the Course

**Free Zero to SOC Analyst playlist:**
https://www.youtube.com/playlist?list=PLCA_oueymC_WElNXo-vGxKvoa0Bh4NC2b

## 🎯 What You'll Be Able to Do

- Get fluent in your SIEM and stop second-guessing where to click
- Write basic searches in your chosen query language
- Filter by time, host, user, and event type
- Pivot from one log entry to related activity

## 🛠 Our Tool of Choice

This section uses **Elastic (the ELK Stack)** as the SIEM. If you'd rather use Splunk, you're welcome to. Both are industry standard, and the core skills (searching, filtering, pivoting, dashboards) transfer cleanly between them.

## 📚 Lessons

Work through these in order.

1. [What Is a SIEM and Why Do We Use One?](./01-what-is-a-siem.md) — The central place all your logs land, the four things it gives you, and why no SOC works without one.
2. [Anatomy of a Log](./02-anatomy-of-a-log.md) — The four-part framework for reading any log from any system, anywhere.
3. [The Elastic Stack](./03-the-elastic-stack.md) — What the ELK Stack actually is, how your logs get into it, and where you sit as the analyst.
4. [Tour of Kibana](./04-tour-of-kibana.md) — Where the things that matter live in Discover, and why the time picker is the control that rules everything.
5. [Load the Course Dataset](<./Load the course dataset>) — One-time setup: load the `soc-lab` dataset into your own Kibana so every lab runs against the exact same logs.
6. [Searching Logs with KQL](./05-kql-basics.md) — The query grammar that answers real investigation questions from a blank search bar.

## 📁 What's in This Folder

- The lesson files above (companion notes to each video)
- `logs/` — the sample dataset you load during setup
- `images/` — diagrams referenced in the lessons
