# Logon Events: 4624, 4625, and 4634

The companion file for the video. This is the map, not the lecture. The reasoning lives on camera. Load the dataset, run the queries, and answer the questions at the bottom.

Logons are one of the busiest events in a Windows environment, and they are where most investigations actually start. Learn to read them like a book and the abnormal ones jump out.

---

## The three event IDs

Three IDs carry most of the logon story. This is the vocabulary for the whole lesson.

| Event ID | Meaning | Plain English |
|---|---|---|
| **4624** | Successful logon | Somebody or something authenticated. |
| **4625** | Failed logon | Somebody tried and got rejected. |
| **4634** | Logoff | The session ended for that user. |

These tell you **what** happened. They do not tell you the context around it. Context is what you get paid for as an analyst.

---

## The one field that changes everything: Logon Type

Both 4624 and 4625 carry a **Logon Type** field. It tells you *how* and *where* the logon happened, which is what turns a boring line into an interesting one.

| Type | Name | What it means | Analyst note |
|---|---|---|---|
| **2** | Interactive | Hands on keyboard, in front of the machine | Normal at a user's own workstation. |
| **3** | Network | Coming across the network (file shares, remote access) | The one to watch. Lots of activity hides here. |
| **5** | Service | A service account starting on schedule | Very common. Backups, monitoring, heartbeats. |
| **7** | Unlock | Someone unlocked the screen | Person came back to their desk. |
| **10** | Remote Interactive | An RDP session | Admins and attackers both love this. Lateral movement. |
| **11** | Cached Interactive | Logon with cached credentials | A domain laptop logging in while off the domain. |

> In this section's dataset you will see types **2, 3, 5, 7, and 10**. Type 11 is here for completeness; it does not appear in this sample set.

**Same event ID, different story.** A type 7 unlock at 9 a.m. on someone's own workstation is a person getting coffee. A type 3 network logon to a domain controller from a machine you do not recognize is something you look into. Same 4624. The fields make the difference.

---

## Normal before weird

You cannot spot suspicious until you know boring. A normal day has a rhythm:

- A **morning wave** of interactive logons (type 2) as people arrive, tapering off as they leave.
- **Service accounts** (type 5) authenticating all day and night to do their job. `svc_backup` is the loud one in this data.
- **Network logons** (type 3) to the file server and app server from internal workstations as people reach shared resources.
- A **handful of failed logons** (4625, type 2) where someone fat-fingered a password. Two or three tries, ten to fifteen seconds apart, then a success. That is human.

What is not human: twenty attempts in five seconds, or one source trying many different accounts in a row. That is a machine, and that is where you start reading.

---

## Lab walkthrough (follow along in Kibana)

Load `Logs/infosecwizard-section2-windows-dataset.ndjson` into your stack first. The field names below match that dataset (ECS mappings from Winlogbeat). Match the host casing exactly.

### Step 1: Successful logons (4624)

```
event.code : "4624"
```

There are about 250 of these. Add columns so you can scan instead of scroll:

- `user.name`
- `winlog.event_data.LogonType`
- `winlog.event_data.IpAddress`
- `host.name`

Notice the pattern: `svc_backup` type 5 all over the place, interactive type 2 at workstations, type 3 to `FS01` and `APP01` from internal `10.0.0.x` addresses.

### Step 2: Read a network logon like a sentence

Filter to network logons that carry a real source IP:

```
event.code : "4624" and winlog.event_data.LogonType : "3" and not winlog.event_data.IpAddress : "-"
```

Take one row and read it out loud: *"Someone at source IP `10.0.0.52` logged into the `carter.l` account on host `FS01` over the network."* Source, account, target host. That is the whole skill. Do it until it is automatic.

### Step 3: Failed logons (4625) and finding the break in the rhythm

```
event.code : "4625"
```

Most are type 2 at workstations, one or two at a time. Normal. Then sort by time and look at **2026-06-17 around 13:12**. One source, `203.0.113.77`, fails against a long list of different accounts on `DC01` in under a minute. That is not a person forgetting a password. That is a spray. Everything after it is worth following.

---

## Quick reference

```
Logon event IDs
  4624 ... successful logon
  4625 ... failed logon
  4634 ... logoff

Logon types you will see here
  2  ... interactive (at the keyboard)
  3  ... network (across the wire)  <- watch this one
  5  ... service (scheduled/automatic)
  7  ... unlock
  10 ... remote interactive (RDP)

Key fields
  winlog.event_data.LogonType       -> how / where
  winlog.event_data.TargetUserName  -> the account (also user.name)
  winlog.event_data.IpAddress       -> source ("-" when local)
  host.name                         -> the target system

Golden rule
  The event ID tells you WHAT. The fields tell you whether to care.
```

---

## Knowledge check

Decide what each one is and whether it is normal or worth a look. Answer key at the bottom.

1. A 4624, type 5, from `svc_backup` at 2 a.m. on the app server.
2. A 4624, type 3, to `DC01` from an IP address that is not on your network.
3. Three 4625s, type 2, from one user at their own desk, followed by a 4624.
4. Fourteen 4625s, type 3, from one source IP against fourteen different accounts on `DC01` inside one minute.
5. A 4624, type 10, to a server in the middle of the night from a workstation that has never connected there before.
6. A 4624, type 7, on a user's own workstation at 9 a.m.

### Answer key

| # | What it is | Verdict |
|---|---|---|
| 1 | Service account logon | Normal. Service accounts run around the clock. Know your baseline before you flag it. |
| 2 | Network logon to a DC from outside | Suspicious. A DC should not take network logons from unknown external sources. |
| 3 | A user fat-fingering a password | Normal. A few failures then a success is human. |
| 4 | A password spray | Suspicious. One source, many accounts, machine speed. This is an attack. |
| 5 | Off-hours RDP from a new source | Suspicious. Type 10 plus an unusual source and time is classic lateral movement. |
| 6 | Screen unlock | Normal. Someone came back to their desk. |

---

## Additional exercises (homework)

1. **Build your normal.** Pull the type 5 logons for `svc_backup` and describe its pattern in one or two sentences. What would make a `svc_backup` logon abnormal?
2. **Read three out loud.** Pick three type 3 network logons and write each as a plain sentence: source IP, account, target host.
3. **Find the spray.** Reconstruct the `203.0.113.77` event on `DC01`: how many accounts, over what time span, and what tells you it is automated rather than human?
4. **Follow the thread.** After the spray, does that activity lead anywhere? Pivot from the logon story on `DC01` into what runs next (you built the process-creation skills in the next lesson). Write two or three sentences on what you think happened.
5. **Sort the noise from the signal.** Across all 4625s, separate the ordinary fat-finger failures from the ones that matter. What field or pattern did you use to tell them apart?

Drop your answers in the Discord `#lab-answers` channel and compare with how others read the same data.

---

## Next

You can now read a logon and tell whether to care. Next we follow the thread from "who got in" to "what they did once inside" with process creation logs.

- Free Zero to SOC Analyst course: https://www.youtube.com/playlist?list=PLCA_oueymC_WElNXo-vGxKvoa0Bh4NC2b
- Questions as you work through it: https://discord.gg/XjHJFp4KSm

_Lab environment only. Never run any of this on production-adjacent networks._
