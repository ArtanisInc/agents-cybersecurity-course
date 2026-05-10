# SSH Brute Force Triage

## Purpose

This runbook helps analysts triage SSH brute force alerts.

## Steps

1. Count failed login attempts.
2. Identify source IP addresses.
3. Identify targeted usernames.
4. Check if any successful login happened after the failed attempts.
5. Verify whether the successful login came from a trusted internal IP.
6. If external failed attempts are high but no external successful login occurred, keep severity at medium.
7. If a successful login from an external suspicious IP occurred, escalate to high.
8. Recommend blocking suspicious IPs only after human validation.