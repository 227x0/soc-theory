# Ports Quiz — Attempt 01 (2026-11-24)

**Result: PASS — 21/21 ports + 5/5 SOC-risk questions (threshold 17/20).**

## Round 1 & 2 — port → service (21/21)

All correct: 20/21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 67/68 DHCP,
80 HTTP, 110 POP3, 143 IMAP, 161 SNMP, 389 LDAP, 443 HTTPS, 445 SMB,
636 LDAPS, 1433 MSSQL, 3306 MySQL, 3389 RDP, 5432 PostgreSQL, 6379 Redis,
8080 alt-HTTP.

Mail trio mnemonic used: 25 delivers (server→server), 110 fetches to device,
143 reads on server.

## Lightning round — "what's wrong with this exposed port?" (5/5)

| Port | Answer (summary) | Grade |
|---|---|---|
| 3389 | "Do we even need RDP exposed? Brute-force / password-spray target" | ✅ |
| 6379 | Open Redis DB — unauthenticated data access | ✅ |
| 23 | Telnet has no encryption — creds in plaintext | ✅ |
| 1433 | Exposed MSSQL: either developer misconfig (close it) or a honeypot | ✅ — analyst framing |
| 25 | Open relay → mass spam through our server, blacklisting | ✅ |

## Patterns

- None — full pass. Ports topic **closed**, only spaced repetition from now on.
