# Tour of Kibana

The companion to the video. Open Kibana in one tab and this file in the other. Walk through together, then run the exercise at the bottom.

## 📺 Watch the Video

🎥 [Coming soon — link will be added once the episode publishes]

## 🎯 What You'll Walk Away With

- Knowing where the five things that matter live in Discover
- Knowing the time picker is the most important control and why
- Being able to expand a document and add or remove columns without thinking about it

---

## Quick Reference

The five things in Discover. Bookmark this.

| # | Element | Where it lives | What it does |
|---|---------|----------------|--------------|
| 1 | Data view selector | Top left | Picks which indices you're searching |
| 2 | Time picker | Top right | Sets the time range. **Always set this first.** |
| 3 | Search bar | Middle of the page | Where KQL queries go |
| 4 | Filter pills | Below the search bar | Clickable narrowings of your data |
| 5 | Document table + field list | Main area + left sidebar | Where you actually read and investigate logs |

---

## Walkthrough

Same order as the video.

### Getting to Discover

Hamburger menu (top left) → Discover.

The nav menu has a lot of options. Only two matter right now: **Discover** and **Dashboard**. Ignore the rest for this section.

### 1. Data View Selector (top left)

A dropdown that tells Kibana which indices to show. If your environment has multiple data views, switch between them here. If you only have one, this dropdown will be locked to it. Either way, click around. You won't break anything.

### 2. Time Picker (top right)

**The single most important control in Kibana.**

- Default range is the last 15 minutes.
- If you're searching for something that happened yesterday and finding nothing, the time picker is almost always why.
- First thing you do when you open Discover is set the time range. Every time.

Three ways to set it: quick ranges (last 15m, 1h, 24h, 7d), absolute date ranges, or relative (`now - 7d`).

### 3. Search Bar (middle)

Where KQL (Kibana Query Language) queries go. We cover KQL in its own section.

Simplest possible search for now: type any word and hit enter. Kibana returns every document with that word in any field.

### 4. Filter Pills (below the search bar)

Clickable narrowings of your data. Two ways to add them:

- **Manually:** click "Add filter" and configure
- **From the data:** click a value in the document table or field list and Kibana adds the filter automatically

This is how you'll add filters 90% of the time. Pills can be toggled, inverted, or removed from the bar.

Time range + search + filters all apply at the same time. Your results match all three.

### 5. Document Table + Field List

**Document table** (main area): each row is one log entry. Click the expand arrow on the left of any row to see every field that document contains.

**Field list** (left sidebar): every field in your data view, split into Available and Selected.

- Click the `+` next to any field to add it as a column in the table
- Hover over a field for plus/minus buttons that filter for or against its most common value
- Click any specific value to filter for or against that exact value

### Adding & Removing Columns

- **Add:** click `+` next to a field in the field list
- **Remove:** click the column header in the table → "Remove column"

The table is yours. Customize it for the investigation you're running.

---

## 🧪 Exercise: Tour Discover Yourself

Open Kibana, go to Discover, and find each of the five elements. Tick them off as you go.

- [ ] Found the **data view selector** (top left). How many data views exist in your environment?
- [ ] Found the **time picker** (top right). Set it to "Last 24 hours."
- [ ] Found the **search bar**. Typed a single word, hit enter, got results back.
- [ ] Found the **filter pills** area. Clicked a value in a document to add a filter automatically. Then removed the pill.
- [ ] **Expanded a document** by clicking the arrow on the left of a row. Picked one field name from inside that surprised you.
- [ ] **Added a column** by clicking `+` on a field in the field list, then removed it.

### Share

Post in the [Apprentice Discord](https://discord.gg/XjHJFp4KSm) thread for this episode with the field name from the document you expanded that surprised you. Read what other people posted — their data won't look like yours.

---

## 🔑 Key Takeaways

- 90% of Kibana you don't need to touch. The 10% that matters lives in Discover.
- Five elements: **data view selector, time picker, search bar, filter pills, document table + field list**.
- The **time picker** is the most important control. Always set it first.
- The **field list** lets you filter and add columns without writing KQL.

---

## ➡️ What's Next

You can navigate Discover. The next step is learning the language Kibana uses to search through your logs, so you can ask any question you want.

[← Back to Section 1 Overview](./README.md) | [Next →](./05-kql-basics.md)
