# Course Roadmap: The 8-Week SOC Analyst Foundation

Welcome. This document is your full map of the course. It explains the why behind the sequencing, what you'll learn each week, and how to know when you're ready to move on.

## Who This Course Is For

You're in the right place if:

- You're trying to break into cybersecurity as a SOC analyst
- You're early in a SOC role and want to fill in the gaps no one taught you
- You're switching from IT, networking, or another tech field
- You've watched a hundred YouTube videos but still don't feel job-ready

You do not need a degree, a certification, or a background in cyber. You do need a computer, a working internet connection, and roughly 4-6 hours per week.

## How the Course Works

The course runs for 8 weeks. Each week, new content drops on YouTube and the matching folder in this repo goes live with everything that supports the lessons.

Every week includes:

- **One or more video lessons** that teach the concept and walk through the tools
- **At least one lab** (some weeks have more) in the matching `section-XX-topic/lab.md` file so you actually apply what you learned
- **A resources list** in `resources.md` for further reading and tool references
- **Roaming office hours** in the Apprentice Discord. Drop in to work through tough questions live. Keep an eye on the Discord for the next session.

Budget about 4-6 hours per week. One to watch, one to take notes, two to three to do the lab and explore further. Section 8 is heavier because it's the capstone.

## Why This Sequence

The order is deliberate. Section 1 builds the muscle (SIEM fluency) that every later section leans on. Sections 2-6 walk through the data sources and attack patterns a working analyst sees every day, starting with the most common (Windows logs) and stepping through the most common attack categories. Section 7 flips the lens from consuming detections to writing them, which is the seniority jump most courses skip entirely. Section 8 forces you to synthesize all of it in a capstone investigation that doubles as a portfolio piece for job applications.

Don't skip ahead. Each section assumes the one before it.

## Section-by-Section Breakdown

### Section 1: SIEM Fundamentals

**Why it matters:** A SIEM is where you'll spend most of your day. Everything else in this course assumes you can search, filter, and pivot through data fluently. Get this section right and the next seven are dramatically easier.

**What you'll be able to do:**
- Get fluent in your SIEM and stop second-guessing where to click
- Write basic searches in your chosen query language
- Filter by time, host, user, and event type
- Pivot from one log entry to related activity
- Save searches and build a starter dashboard

**Lab focus:** Search through sample log data to answer real investigation questions.

**Our tool of choice:** This course will focus on **Elastic (ELK Stack)**, but if you'd rather use Splunk, you're free to do so. Both are industry standard, and the core skills (writing searches, filtering, pivoting, building dashboards) transfer cleanly between them. The labs are built around Elastic, but the concepts apply either way.

### Section 2: Windows Logs

**Why it matters:** Windows is the most common endpoint in enterprises, which means Windows event logs are the most common thing you'll be reading. Knowing the key event IDs cold is non-negotiable.

**What you'll be able to do:**
- Recognize the event IDs that actually matter (and ignore the noise)
- Follow a login chain from authentication through process creation
- Read process creation events (4688) and Sysmon data
- Spot suspicious command-line activity

**Lab focus:** Walk through a Windows incident using event logs in your SIEM.

### Section 3: Phishing Investigations and Attack Chains

**Why it matters:** Phishing is the #1 initial access vector. The majority of SOC tickets start with someone clicking something they shouldn't have. This section teaches you how to handle the most common ticket type you'll ever see.

**What you'll be able to do:**
- Analyze email headers and extract IOCs
- Understand SPF, DKIM, and DMARC well enough to spot spoofing
- Trace activity from the email click to endpoint compromise
- Map the steps to MITRE ATT&CK techniques

**Lab focus:** Investigate a phishing email from raw headers through to post-compromise activity.

### Section 4: Authentication Attacks

**Why it matters:** Identity is the new perimeter. Active Directory attacks are everywhere, and most analysts can't tell Kerberoasting from a brute force attempt. After this section, you can.

**What you'll be able to do:**
- Spot brute force and password spraying patterns in logs
- Recognize Kerberoasting and AS-REP roasting
- Understand golden and silver ticket attacks well enough to explain them
- Detect signs of lateral movement using credentials

**Lab focus:** Detect a Kerberos-based attack in real log data.

### Section 5: Malicious PowerShell Analysis

**Why it matters:** PowerShell is the attacker's tool of choice on Windows. If you can't read obfuscated PowerShell, half your incidents will go unsolved.

**What you'll be able to do:**
- Decode base64-encoded PowerShell commands
- Recognize common obfuscation patterns
- Understand PowerShell script block logging and transcription
- Spot LOLBins (Living Off the Land Binaries) being abused

**Lab focus:** Decode and analyze a malicious PowerShell payload.

### Section 6: Network Logs (C2, Exfil, Firewall, WAF)

**Why it matters:** Network logs catch what endpoint logs miss. Beaconing, command-and-control callbacks, data exfiltration. Without this section, you'll only see half the attack.

**What you'll be able to do:**
- Read firewall and proxy logs
- Spot beaconing patterns and common C2 channels
- Recognize signs of data exfiltration
- Analyze WAF logs for web application attacks

**Lab focus:** Hunt for C2 traffic and exfil in network log data.

### Section 7: Detection Logic (Reading and Writing Rules)

**Why it matters:** Junior analysts consume alerts. Senior analysts write them. This section is the bridge between the two. Most courses skip it entirely. That's why most courses don't get people promoted past Tier 1.

**What you'll be able to do:**
- Read existing detection rules in Sigma, KQL, or SPL
- Understand the structure of a detection (what, where, when, false positives)
- Write a detection from a threat intel report
- Test your detection against sample data and tune out false positives

**Lab focus:** Write a Sigma rule for a specific attack technique and validate it.

### Section 8: Capstone Investigation

**Why it matters:** This is where everything comes together. You investigate a full incident on your own and produce a written report. The output isn't just a course completion. It's a portfolio piece you can show in interviews.

**What you'll be able to do:**
- Run a full investigation independently using everything from Sections 1-7
- Construct an accurate incident timeline
- Write a complete incident report with an executive summary and technical findings
- Present findings the way you would to leadership

**Lab focus:** Investigate a full incident scenario end-to-end and produce a deliverable report.

Budget more time for this one. Plan on 6-10 hours.

## After Section 8

You'll have:

- A working knowledge of SIEM, Windows logs, phishing, AD attacks, PowerShell, network logs, and detection writing
- A finished capstone investigation report you can show in interviews
- A clear sense of what to study next (advanced threat hunting, specific SIEM certs, incident response specializations)

This is a foundation, not a finish line. But it's the foundation most working SOC analysts wish they'd had on day one.

## Tracking Your Progress

The simplest way to track yourself is to commit your completed labs to your own fork of this repo. By the end of Section 8, you'll have a public GitHub portfolio showing eight weeks of hands-on cybersecurity work. That alone makes you stand out in a stack of resumes that all say "studying for Security+."

If GitHub feels like too much overhead at the start, just keep a running log in a single document. The goal is evidence of work, not perfect tooling.

## Need Help

**Join the Discord:**
https://discord.gg/XjHJFp4KSm

Drop in any time during the week to ask questions or share progress. Roaming office hours happen inside Discord. Keep an eye out for the next session.

Let's get to work.
