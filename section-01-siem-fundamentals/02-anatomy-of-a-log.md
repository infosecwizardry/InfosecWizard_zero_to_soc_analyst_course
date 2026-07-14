# Anatomy of a Log

A log is just a record of something that happened, with a timestamp. Once you can read one, every search you'll ever write in a SIEM makes sense. This section gives you the four-part framework that works on every log from every system, anywhere.

## 📺 Watch the Video

[![Watch on YouTube](https://img.youtube.com/vi/tvWzYVuL_34/maxresdefault.jpg)](https://youtu.be/tvWzYVuL_34)

## 🎯 What You'll Walk Away With

- Identifying the four universal parts of any log entry
- Reading logs from Windows, Linux, and web servers using the same mental framework
- Spotting the difference between structured and unstructured logs
- Pulling useful information out of raw log data without being intimidated by it

---

## 📖 Lesson Notes

### What a Log Actually Is

Strip away the technical baggage. A **log entry** is just a record of something that happened, with a timestamp.

Every system you use is generating them constantly. Your phone is logging. Your laptop is logging. The wifi router you're connected to is logging. The website you visited five minutes ago has a log entry for your visit sitting on a server somewhere.

In security, those events come from servers, endpoints, firewalls, applications, and the cloud. The system writes them down. We read them.

### The Four Universal Parts of Every Log

Every log entry, no matter where it came from, has roughly four parts:

1. **Timestamp:** when it happened
2. **Source:** where it came from (which system, application, or device)
3. **Event type:** what kind of event it is (a login, a network connection, a file write, an error)
4. **Fields:** the specific details (who did it, from where, with what)

This framework holds whether you're looking at a Windows event, a web server log, a firewall log, a cloud audit log, or anything else. Once you can spot the four parts, you can read any log.

### Reading Real Logs

The same kind of event looks completely different depending on the source. Let's break down three samples.

**Sample 1: An Apache web access log**

```
198.51.100.42 - - [12/Mar/2026:08:23:47 -0500] "GET /admin/login.php HTTP/1.1" 200 1547 "https://example.com/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
```

- **Timestamp:** `12/Mar/2026:08:23:47`
- **Source:** the web server (the IP `198.51.100.42` is the client; the log itself came from the server's access log)
- **Event type:** an HTTP GET request to `/admin/login.php`, returning a 200 status code (success)
- **Fields:** client IP, response size in bytes, referring URL, user agent string

**Sample 2: A Linux SSH auth log**

```
Mar 12 14:23:47 webserver01 sshd[12345]: Accepted publickey for mike from 10.0.1.50 port 49234 ssh2: RSA SHA256:abc123def456
```

- **Timestamp:** `Mar 12 14:23:47`
- **Source:** `webserver01`, specifically the `sshd` service
- **Event type:** an accepted publickey SSH login
- **Fields:** user (`mike`), source IP (`10.0.1.50`), source port (`49234`), authentication method (RSA)

**Sample 3: A Windows Security Event**

```
Event ID: 4624
Date/Time: 3/12/2026 8:23:47 AM
Source: Microsoft-Windows-Security-Auditing
Computer: WORKSTATION42.corp.local
Account Name: msmith
Account Domain: CORP
Logon Type: 10
Source Network Address: 192.168.1.105
```

- **Timestamp:** `3/12/2026 8:23:47 AM`
- **Source:** `WORKSTATION42` running the Microsoft-Windows-Security-Auditing service
- **Event type:** Event ID 4624 (a successful logon, in Windows terms)
- **Fields:** account name (`msmith`), domain (`CORP`), logon type (`10` = remote interactive), source IP (`192.168.1.105`)

Three different formats. Three different systems. Same four-part framework.

### Structured vs Unstructured Logs

Logs come in two basic shapes:

- **Unstructured logs** are raw text. The Apache log above is unstructured. You can parse it by eye or with regex, but a computer has to work to pull specific values out.
- **Structured logs** (usually JSON) have explicit named fields. Same data, but every value lives in a labeled field like `client_ip` or `http_status`.

The same Apache log entry in structured JSON would look like this:

```json
{
  "timestamp": "2026-03-12T08:23:47-05:00",
  "client_ip": "198.51.100.42",
  "http_method": "GET",
  "url_path": "/admin/login.php",
  "http_status": 200,
  "response_bytes": 1547,
  "referer": "https://example.com/",
  "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
}
```

The good news: modern SIEMs do this conversion automatically. When an unstructured log is ingested into Elastic, the SIEM parses it into structured fields. By the time you see it in Kibana, the messy text line has already become nice clickable, searchable fields.

**The takeaway:** you don't usually have to read raw logs in their original format. The SIEM has already broken them down. But you should know what the originals look like, because during an investigation you may need to explain what the underlying data actually means.

---

## 🧪 Exercise: Three Mystery Logs

Below are three real log entries from three different systems. Your job is to break each one down using the four-part framework, then give a one-sentence read on whether the entry looks normal, suspicious, or somewhere in between.

### Log 1

```
Event ID: 4624
Date/Time: 3/15/2026 9:42:18 AM
Source: Microsoft-Windows-Security-Auditing
Computer: FIN-LAPTOP-04.corp.local
Account Name: jdoe
Account Domain: CORP
Logon Type: 2
Source Network Address: 192.168.10.45
Workstation Name: FIN-LAPTOP-04
```

### Log 2

```
203.0.113.87 - - [15/Mar/2026:23:47:12 -0500] "GET /wp-admin/admin-ajax.php?action=user_dump HTTP/1.1" 200 8743 "-" "sqlmap/1.7.2#stable (https://sqlmap.org)"
```

### Log 3

```
Mar 15 22:47:33 webserver01 sshd[8432]: Accepted password for admin from 198.51.100.42 port 49284 ssh2
```

### Your Task

For each log entry, identify:

1. **The timestamp:** when it happened
2. **The source:** what system or application produced it
3. **The event type:** what happened
4. **At least three fields:** specific details visible in the entry
5. **Your read:** does this look normal, suspicious, or in between? Why?

### Share Your Work

Post your breakdown in the [Apprentice Discord](https://discord.gg/XjHJFp4KSm) or in the YouTube comments. Then read at least two other people's breakdowns. You'll find details you missed and you'll disagree with someone's "normal vs suspicious" read on at least one of the three. That's the point.

---

## 🔑 Key Takeaways

- Every log entry has four universal parts: **timestamp, source, event type, and fields**.
- The same kind of event looks completely different across systems, but the framework always applies.
- Logs come in two formats: unstructured (raw text) and structured (JSON). Modern SIEMs convert one to the other automatically.
- Once you can read a log, the rest of the course makes sense. Without that, queries are just gibberish.

---

## ➡️ What's Next

Now that you can read a log, the next step is understanding the system that stores and lets you search through millions of them at once.

[← Back to Section 1 Overview](./README.md) | [Next →](./03-the-elastic-stack.md)
