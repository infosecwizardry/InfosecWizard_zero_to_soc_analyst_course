# A Tour of Windows Event Logs

The companion file for the video. This is the map, not the lecture. The reasoning lives on camera. This page is here to scan and come back to.

No lab this time. It's the overview. Hands-on work starts when we take each category apart on its own.

---

## The idea

Event Viewer feels like a firehose. Screen after screen, mostly noise, no obvious place to start.

You don't memorize it. Almost everything worth reading is one of a small set of activities. Learn to spot which one you're looking at and the pile starts to sort itself.

---

## Channels: how Windows files its logs

- **Security:** logons, account changes, privilege use. Where you'll spend most of your time.
- **System:** services, drivers, reboots.
- **Application:** software writing its own logs.
- **PowerShell:** PowerShell activity (`Microsoft-Windows-PowerShell/Operational`).
- **Sysmon:** richer detail once it's installed (`Microsoft-Windows-Sysmon/Operational`).

Knowing the drawers isn't the same as knowing what to read. So organize by what happened, not by which drawer it landed in.

---

## The map: seven kinds of activity

Each one becomes its own deep dive.

| # | Activity | What it logs | Why we care |
|---|----------|--------------|-------------|
| 1 | Someone logged in | Who signed in, from where, and how | Trace a user across their whole life on the network. |
| 2 | A program ran | What ran, its command line, and what launched it | See what's running on a system and whether it's tied to anything malicious. |
| 3 | Someone ran PowerShell | The commands that were run | Attackers lean on PowerShell for its reach. This is where you see everything they ran. |
| 4 | Something talked to the network | A process and the connections it made | Watch how processes reach other systems on the network or out to the internet. |
| 5 | An account or group changed | Accounts created or deleted, resets, group membership | A common persistence move. Watch for the account nobody asked for. |
| 6 | Something set up to stick around | Scheduled tasks and services | Persistence. Spot the task or service planted to survive a reboot. |
| 7 | Files or settings changed | Files written, registry keys set | Malware staging a payload or wiring itself into a startup key. USB surfaces here too. |

Seven kinds of activity, not a thousand IDs to memorize.

---

## The mindset: normal before weird

You can't spot suspicious until you know boring.

Every row above is mostly ordinary traffic. People log in every morning. Programs run all day. Accounts get created with every new hire. Hunt for bad without knowing normal and you either chase everything or miss the one that counts.

So every deep dive goes the same way: what this looks like when nothing's wrong, then what makes you look twice.

---

## One thing before you go looking

Windows doesn't log the good stuff by default. The full command line of a program, for example, is off until someone turns it on. What you can see depends on how the place was set up before you got there. First question in a new job: what can I even see here?

**Sysmon** is the free add-on that fixes a lot of that. It records the same activity with more detail, including network connections tied back to the process that made them. As we go, you'll see the plain Windows event first, then the sharper Sysmon version.

---

## Knowledge check

For each situation, decide which of the seven categories it belongs to. A few could fit more than one. When that happens, naming both and saying why is the better answer. Answer key is at the bottom.

1. An analyst needs to see every machine a single user account touched during one shift.
2. A finance workstation reaches out to an IP address in another country overnight.
3. A brand new account is added to the Domain Admins group.
4. Microsoft Word launches a command shell.
5. A service is installed that runs a binary out of a temp folder.
6. An encoded command runs and pulls a file down from the internet.
7. A registry key is changed so a tool starts automatically at every login.
8. An account that was disabled last month is suddenly re-enabled.
9. You want to find out what launched a suspicious executable.
10. A host makes identical small connections to the same external server every sixty seconds.
11. Files are copied onto a USB drive that was just plugged in.
12. A scheduled task appears that runs every time the machine boots.
13. An account signs in over RDP from a workstation it has never connected from before.

### Answer key

| # | Category | Note |
|---|----------|------|
| 1 | Someone logged in | Following one account's logons across hosts is how you trace a user's activity. |
| 2 | Something talked to the network | The connection itself is the event. Where it went is the tell. |
| 3 | An account or group changed | Membership change. The group being Domain Admins is what makes it loud. |
| 4 | A program ran | A document spawning a shell is the parent-child relationship that matters. |
| 5 | Something set up to stick around | Persistence via a service. The temp-folder path is the giveaway. |
| 6 | Someone ran PowerShell, and something talked to the network | Two categories. The command is PowerShell activity; the download is network activity. |
| 7 | Files or settings changed, and something set up to stick around | Two categories. A run key is a settings change and a persistence mechanism. |
| 8 | An account or group changed | Re-enabling a dormant account is an account change worth questioning. |
| 9 | A program ran | The parent process tells you what launched it. |
| 10 | Something talked to the network | Regular, identical outbound connections are a classic beaconing pattern. |
| 11 | Files or settings changed | File activity, and the category where USB tends to surface. |
| 12 | Something set up to stick around | Persistence via a scheduled task set to survive reboots. |
| 13 | Someone logged in | A logon from an unusual source is normal activity in an abnormal context. |

---

## Next

Pick any row and that's a deep dive waiting. Each starts with normal, then moves to what stands out.

- Free Zero to SOC Analyst course: https://www.youtube.com/playlist?list=PLCA_oueymC_WElNXo-vGxKvoa0Bh4NC2b
- Questions as you work through it: https://discord.gg/XjHJFp4KSm

_Lab environment only. Never run any of this on production-adjacent networks._
