# SPL Query Reference — Cheat Sheet

Running list of useful Splunk search queries. Updated as I learn more.

---

## Basic Search Syntax

```spl
index=main sourcetype=syslog
```
Search all syslog events in the main index.

```spl
index=main earliest=-24h latest=now
```
Search the last 24 hours.

```spl
index=main source="/var/log/auth.log" "Failed password"
```
Search for failed SSH login attempts.

---

## Field Commands

```spl
index=main | fields src_ip, dest_ip, action
```
Only show these specific fields.

```spl
index=main | table src_ip, dest_ip, status
```
Display results as a clean table.

```spl
index=main | rename src_ip AS "Source IP"
```
Rename a field for cleaner display.

---

## Filtering

```spl
index=main status=404
```
Find all 404 errors.

```spl
index=main | where src_ip="192.168.1.10"
```
Filter by specific IP.

```spl
index=main action=blocked | stats count by src_ip
```
Count blocked events by source IP — useful for finding attackers.

---

## Stats and Aggregation

```spl
index=main | stats count by src_ip
```
Count events grouped by source IP.

```spl
index=main | stats count by user | sort -count
```
Find most active users, sorted by count descending.

```spl
index=main failed | stats count by user | where count > 5
```
Find users with more than 5 failed attempts — brute force detection.

---

## SOC Analyst Queries

```spl
index=main "Failed password" | stats count by src_ip | sort -count
```
Find IPs with the most failed SSH logins — potential brute force.

```spl
index=main | stats dc(src_ip) AS unique_ips by dest_ip
```
Count unique source IPs hitting each destination — detect port scanning.

```spl
index=main status=200 | timechart count by src_ip
```
Chart traffic over time by source IP — detect spikes.

---

*Updated as new queries are learned through Splunk training and TryHackMe SOC Level 1 path.*
