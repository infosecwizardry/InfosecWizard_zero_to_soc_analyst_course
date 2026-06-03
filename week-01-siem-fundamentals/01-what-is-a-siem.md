# Episode 1: What Is a SIEM and Why Do We Use One?

Welcome to Episode 1 of Zero to SOC Analyst. This is the foundation. Everything in the next seven weeks builds on what you learn here.

## 📺 Watch the Video

🎥 https://youtu.be/ctLU2CCLMqo

## 🎯 Learning Objectives

By the end of this section, you'll be able to:

- Explain what a SIEM is and what problem it solves
- Identify the four core benefits a SIEM provides
- Recognize the difference between investigating with and without a SIEM
- Answer the most common SOC analyst interview question: "What is a SIEM?"

---

## 📖 Lesson Notes

### What Is a SIEM?

SIEM stands for **Security Information and Event Management**. Most people just say "SIEM," and pronounce it to rhyme with "team."

In plain English, a SIEM is the single place where all your logs live. Logs from your servers, your endpoints, your firewall, your cloud accounts, your email gateway, every security tool you run. The SIEM pulls all of those logs in, organizes them, and gives you one interface to search across all of them at once.

Think of it like Google for your entire environment. Instead of logging into thousands of different systems to look around, you log into one. You type a search. You get answers.

### The Problem a SIEM Solves

Imagine an alert hits your queue at 2 AM: "Suspicious login attempt on workstation 42."

That workstation is one of thousands of hosts in your environment. There are domain controllers, firewalls, EDRs, email gateways, and cloud workloads, all generating logs right now. The information you need to solve this alert is scattered across every one of them.

**Without a SIEM, your investigation looks like this:**

1. RDP into workstation 42 to check the Windows event log
2. Remote into the domain controller to see DC-side authentication
3. Log into the firewall to check network activity at that time
4. Check the EDR console
5. Check email logs for a possible phishing entry point

Five systems. Five separate logins. Five different query languages. Hours of clicking. SOC analysts call this **swivel-chair analysis**.

**With a SIEM, your investigation looks like this:**

1. Open the SIEM
2. Search for the workstation 42 hostname in the last hour
3. Every log entry from every system that mentions that workstation comes back, in order, in one place

Five minutes of searching instead of five hours of clicking. That's the SIEM superpower.

### The Four Pillars of What a SIEM Gives You

A SIEM gives you four things you do not get without one. Memorize these. They're the answer to "what does a SIEM do?" in every SOC interview.

**1. Speed**

Searches that would take hours of clicking through different systems take seconds in a SIEM. Time matters in security. Every minute an attacker is undetected is a minute they're moving deeper.

**2. Correlation**

When all your logs are in one place, you can ask questions that span every system at once. "Show me every login from this IP across every server in the last 24 hours." Without a SIEM, that question is basically unanswerable. With a SIEM, it's one search.

**3. History**

SIEMs hold onto logs for weeks, months, or years, depending on configuration. When you discover a breach today, you can look back at what happened three months ago. Without a SIEM, individual system logs are probably long gone.

**4. Integrity**

This one doesn't get talked about enough.

When an attacker compromises a host, one of the first things they try to do is cover their tracks. Wipe the local event log. Delete records. Modify what's stored on the machine.

But if your SIEM is set up correctly, those logs were shipped off the host the moment they were created. The SIEM already has its copy before the attacker even gets there.

That makes the SIEM your **source of truth**. The logs sitting on a compromised machine can no longer be fully trusted. The attacker may have edited them. But the SIEM copy is untouched, sitting in a system the attacker probably can't reach.

For an analyst, this is huge. The SIEM isn't just convenient. It's often the only version of what really happened that you can actually trust.

### Where We're Going in This Course

Throughout Zero to SOC Analyst, we're going to use **Elastic** (also known as the ELK Stack) as our SIEM. You're welcome to follow along with Splunk if you prefer. Both are industry standard, and the core skills transfer cleanly between them.

This week is dedicated to one thing: getting you fluent in the SIEM itself. Searching, filtering, pivoting, dashboards. The next seven weeks all assume you can do this.

---

## 🧪 Exercise: The 2 AM Login

It's Monday morning. You arrive at work, grab your coffee, and pull up your queue. The first ticket of the day reads:

> "User Sarah Johnson (Marketing, Denver office) reports her password was changed without her knowledge sometime over the weekend. The IT helpdesk reset her password this morning. Sarah's manager checked the Microsoft 365 activity log and saw a successful login from a public IP address in Boston at 2:14 AM Sunday morning. Sarah lives in Denver and insists she was asleep. Please investigate."

Welcome to your first incident.

### Your Task

You don't have a SIEM yet. That comes next. For now, just think.

**If you had access to every log in this organization, which systems would you want to check to investigate this incident? For each system, what specifically would you be looking for?**

Aim for at least 5 systems. For each one, write a single sentence explaining what you'd check there.

Example to get you started:

- **Microsoft 365 sign-in logs:** confirm the 2:14 AM login details (IP address, device, user agent). Look for any other unusual sign-ins around the same time.

Keep going from there.

### Share Your List

When you're done, post your list in the [Apprentice Discord](https://discord.gg/XjHJFp4KSm) or in the YouTube comments. Then read what at least three other people came up with. You'll find systems you didn't think of, and that's the whole point.

### What This Exercise Is Really Teaching You

Every system you listed is a log source you'd want centralized in a SIEM. The fact that you needed to think about five or more separate places to investigate one incident is exactly the swivel-chair problem from the video.

Now imagine doing this for real without a SIEM. Logging into each of those systems separately. Running searches in five different query languages. Copying timestamps into a spreadsheet to piece together a timeline. Hours of clicking.

Now imagine doing it with a SIEM, where a single search across every log source gives you the answer in seconds.

That's the difference. That's why this job needs a SIEM.

---

## 🔑 Key Takeaways

- A SIEM is **Security Information and Event Management**: the central place where all your logs land.
- It gives you four things: **Speed, Correlation, History, and Integrity**.
- Without a SIEM, you do swivel-chair analysis. With one, you do real investigation.
- The SIEM is often your **only trustworthy source of truth** during an incident, because logs are shipped off the host before an attacker can tamper with them.

---

## ➡️ What's Next

Now that you know what a SIEM is and why it exists, the next step is understanding what's actually inside it. That means logs. In Episode 2: Anatomy of a Log, we'll break down what a log entry actually contains, how to read one, and why structured logs are an analyst's best friend.

[← Back to Week 1 Overview](./README.md) | [Next: Anatomy of a Log →](./02-anatomy-of-a-log.md)
