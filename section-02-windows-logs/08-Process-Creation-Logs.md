# Process Creation Logs: Event ID 4688 vs Sysmon Event 1

Part of the **Zero to SOC Analyst** series. This is the lab that goes with the video on reading Windows process creation logs. Logon events tell you someone arrived. Process creation logs tell you what they did once they were inside. This is the closest thing you get to watching an attacker work.

> Watch the video first, then work this lab. Load the sample logs, run the queries yourself, and answer the reflection questions at the bottom. Drop your answers in the Discord.

---

## What you will be able to do

- Explain the difference between a logon event and a process creation event.
- Read the core fields of a 4688 event and a Sysmon Event 1.
- Know what Sysmon gives you that native 4688 does not.
- Use a file hash as an investigation pivot (VirusTotal and across the network).
- Recognize suspicious parent-child process relationships.
- Spot LOLBins (living off the land binaries) in a log.
- Rebuild a short attack chain in Kibana using PIDs.

---

## The one-line mental model

**Logon = the front door opening. Process creation = the security camera footage of what happens in the room.**

Process creation logs capture every program that started, what launched it, and the exact command line that ran. When you work an incident, this is often one of the first places you go.

---

## The two sources

Process creation shows up in two places. Which one you get depends entirely on the environment you land in. Some shops have one, some have both, some have none. Most have at least one.

| Source | Event ID | Notes |
|---|---|---|
| Native Windows Security log | **4688** | On by default in many builds, but command line logging must be enabled via GPO to be useful. |
| **Sysmon** | **Event ID 1** | An enhanced version of the same idea. Its own log channel. Adds fields 4688 does not have. |

If neither is present, your EDR almost certainly logs something very similar with many of the same fields. Learn these and pivoting into an EDR is easy.

---

## Key fields in a 4688 event

| Field | What it tells you | Analyst note |
|---|---|---|
| **New process name** | The executable that actually ran | Watch the path. A common Windows binary running from a user directory is worth a closer look. |
| **Creator process name** | The parent that spawned it | Processes spawning processes is normal. The *relationship* is what matters. |
| **Process command line** | The full arguments the binary ran with | The richest field. Flags, encoded commands, and intent live here. |
| **Subject / account** | Who ran it | Ties back to the logon (4624) events. Not just what ran, but who and on what host. |
| **Token elevation type** | Whether it ran elevated | Privilege context for the action. |

**Who, what, when, where** come from these fields. Your job is to figure out the **why**.

---

## What 4688 leaves out (and Sysmon fills)

Two important gaps in native 4688:

1. **No parent process command line.** You get the parent name, but not the arguments the parent ran with. You can sometimes reconstruct this with queries, but it is not there by default. Sysmon includes it.
2. **No file hash.** Sysmon hashes the image that was executed.

### Why the hash matters

Renaming a file is trivial. Right click, rename, and `malware.exe` becomes `calc.exe`. What renaming does **not** change is the hash. Changing the hash means modifying the file itself.

That gives you a pivot:

- Drop the hash into **VirusTotal**. If `calc.exe` is not signed by Microsoft and Microsoft has never seen it, that is a strong "likely bad" signal.
- Search the hash **across your environment**. Where else did this file run? That is how you scope an incident.

Attackers can modify a file to change its hash, so this is not foolproof. It is one more tool that makes the job easier.

### One more IOC: original file name

`OriginalFileName` is metadata baked in when a file is compiled. If someone renames a binary after compile time, the original file name is often still there. Another useful indicator of compromise.

---

## The blind spot most beginners miss

**Process creation logs only fire when a NEW process starts.**

Once a process is running, it can read files, make network connections, and call APIs to do work **without spawning a new process**. None of that shows up here. These logs give you a large chunk of the picture, not all of it. That is where malware analysis, other forensic artifacts, and other log sources come in.

Do not treat process creation logs as the whole story. No single log is.

---

## Reading process trees: normal vs suspicious

Processes have a rhythm. Learn the normal so the abnormal jumps out.

| Parent -> Child | Verdict | Why |
|---|---|---|
| `explorer.exe` -> `chrome.exe` | Normal | A user double clicked a browser. |
| `WINWORD.EXE` / `EXCEL.EXE` -> `powershell.exe` | Suspicious | Office documents should not launch PowerShell. Usually macro or embedded code. |
| `OUTLOOK.EXE` -> `powershell.exe` / `cmd.exe` | Suspicious | Same story from email. |
| `cmd.exe` -> `whoami`, `net user`, `net group`, `nltest`, `systeminfo` | Suspicious | Classic recon burst. Looks like an attacker enumerating. |

Never judge one event in isolation. **Context is what you get paid for.** Read the whole chain and pull in related logs.

**Caveat:** some admins do things that look sketchy but are just their job. Automation, scripting, and remote tooling can all look ugly. Knowing your environment is what separates a real finding from noise.

---

## LOLBins (Living Off the Land Binaries)

An attacker using **native, signed Windows tools** instead of bringing their own. It helps them blend in, and the hash comes back to a legitimate Microsoft binary, so hash lookups will not flag it. This fools a lot of junior analysts.

Common LOLBins to know:

| Binary | Commonly abused for |
|---|---|
| `powershell.exe` | Download, execute, encoded commands |
| `certutil.exe` | Download and decode files |
| `wmic.exe` | Recon and remote execution |
| `rundll32.exe` | Executing code via DLLs |

When an attacker goes this route, the binary name and hash mean nothing. **The context of how the binary is used** is the tell. Read the command line.

---

## Lab walkthrough (follow along in Kibana)

Load the sample logs from this repo into your stack first: `Logs/infosecwizard-section2-windows-dataset.ndjson`. The field names below match that dataset (ECS mappings from Winlogbeat). Match the host casing exactly.

### Step 1: 4688 process creates on the domain controller

```
event.code : "4688" and host.name : "DC01"
```

Add columns so you can scan instead of scroll:

- `process.name`
- `process.command_line`
- `user.name`

Look at what `irwin.t` is doing. You should see a recon burst: `whoami /all`, `net user /domain`, `net group "Domain Admins" /domain`, `nltest`, `systeminfo`, `ipconfig`, `tasklist`, then an encoded PowerShell command.

### Step 2: Sysmon Event 1 on the file server

```
event.code : "1" and host.name : "FS01"
```

Open an event and add:

- `process.parent.command_line`
- `process.pid`
- `process.parent.pid`

Sort by time, earliest first, and order your columns **parent first, then child** so you can read the logical flow. Walk the chain: an encoded PowerShell command kicks off, a child process runs what looks like a credential-dumping command, then `7z.exe` archives files from a share for exfil.

### Step 3: Pivot

- Use the **PID** to tie a parent to its children when a parent has several.
- Take a suspicious **hash** and check it in VirusTotal and across other hosts.

> Note: the hashes in this sample data are not real. It is synthetic data built to teach the workflow, not for live hash lookups.

---

## Quick reference

```
Process creation event IDs
  Windows Security log ... 4688
  Sysmon ................. Event ID 1

4688 core fields
  New process name        -> what ran
  Creator process name    -> parent
  Process command line    -> arguments / intent
  Subject / account       -> who ran it
  Token elevation type    -> elevated?

Sysmon adds
  Parent command line
  File hash (SHA256, etc)
  Original file name

Golden rule
  New process only. No new process = no log here.
```

---

## Reflection questions

Answer these in your own words. If you cannot, rewatch that section.

1. In one sentence each, what is the difference between a logon event and a process creation event?
2. You see `notepad.exe` running from `C:\Users\jsmith\AppData\Local\Temp\`. What about that is worth a second look, and why?
3. Why is a file hash a more reliable indicator than a file name?
4. Name two things a running process can do that will NOT generate a process creation log. Why does that matter to your investigation?
5. An analyst sees `WINWORD.EXE` spawn `powershell.exe`. Walk through what likely happened and what you would pull next to confirm it.
6. What is a LOLBin, and why does a clean VirusTotal result on a LOLBin not clear the activity?
7. You only have 4688, not Sysmon. Name two fields you are missing and how you might partly reconstruct them.

---

## Additional exercises (homework)

1. **Build your normal.** In the sample data, find three parent-child relationships you would call normal and write one sentence each on why.
2. **Find the recon.** Reconstruct the full recon burst attributed to `irwin.t` on `DC01`, in order. List every command.
3. **Rebuild the chain.** On `FS01`, use PIDs to reconstruct the parent-to-child process chain that ends in an archive being created. Which process created the archive, and what was its parent?
4. **Name the tool.** One event has a command line that looks like a well known credential dumping tool even though the binary is renamed. Which tool, and what in the command line gave it away?
5. **Spot the living-off-the-land activity.** The attacker uses native or renamed binaries instead of obvious malware. Find the encoded PowerShell command and the archiving step in the data, and for each explain why the binary name or hash alone would not flag it. (The `certutil` / `wmic` / `rundll32` LOLBins in the reference table above are common in the real world but do not all appear in this sample set.)
6. **Write the narrative.** In 4 to 6 sentences, tell the story of this attack as if you were briefing a lead: who, what, where, and the order of operations. This is the skill that actually gets you paid.

Drop your answers and the narrative in the Discord `#lab-answers` channel. Compare with how others read the same data.

---

## Going further

- Enable Sysmon in your own home lab with a known-good config and generate your own 4688 and Event 1 logs. Compare the fields side by side.
- Enable command line logging for 4688 via Group Policy and confirm the command line field actually populates.
- Take one benign LOLBin (for example `certutil`) and read up on how defenders detect its abuse. Then look for the benign uses in your own data so you learn the false positives.

---

## Next in the series

You now know how to read process creation logs. Next we keep stacking log sources so you can tell the whole story, not just one chapter of it. Stay tuned.

*Lab environment only. Never run any of this on production-adjacent networks.*
