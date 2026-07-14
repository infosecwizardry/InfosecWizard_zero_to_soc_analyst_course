# Section 2: Windows Logs

Windows is the most common endpoint in the enterprise, which makes Windows event logs the thing you'll read most as an analyst. This section teaches you to stop seeing Event Viewer as a firehose and start reading it by activity: who logged in, what ran, what talked to the network, what changed. Knowing the handful of event types that actually matter is non-negotiable for the job.

## 📺 Watch the Course

**Free Zero to SOC Analyst playlist:**
https://www.youtube.com/playlist?list=PLCA_oueymC_WElNXo-vGxKvoa0Bh4NC2b

## 🎯 What You'll Be Able to Do

- Recognize the event IDs that actually matter (and ignore the noise)
- Follow a login chain from authentication through process creation
- Read process creation events (4688) and Sysmon data
- Spot suspicious command-line activity

## 📚 Lessons

1. [A Tour of Windows Event Logs](./06-Tour-of-Windows-Logs.md) — The map: how Windows files its logs into channels, and the seven kinds of activity worth reading. Scan it, then come back as each category gets its own deep dive.

> **More coming.** This section starts with the overview above. The hands-on deep dives into each activity type publish as the series continues.

## 📁 What's in This Folder

- The lesson files above (companion notes to each video)
- `Logs/` — the Windows sample dataset for this section's labs
