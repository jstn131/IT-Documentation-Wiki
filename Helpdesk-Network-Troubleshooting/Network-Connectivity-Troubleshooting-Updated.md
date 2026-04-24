# No Internet Connectivity — Troubleshooting SOP

**Category:** Network Troubleshooting  
**Tier:** Tier 1 Helpdesk  
**Last Updated:** 2026  

---

## Overview

This SOP covers the step-by-step process for troubleshooting a user who reports they cannot connect to the internet. It covers identity verification, scope assessment, physical checks, DHCP diagnosis, DNS diagnosis, and escalation procedures.

---

## Step 1 — Acknowledge and Verify Identity

Before any troubleshooting begins:

1. Acknowledge the user professionally and let them know you will help resolve the issue
2. **Verify their identity** — ask for their full name, employee ID, or answer to a security question depending on your organization's policy
   - This is required before making any changes on behalf of the user
   - Applies in all environments, especially regulated ones such as healthcare or finance

---

## Step 2 — Determine Scope

Ask the user:

> *"Is this issue affecting just you, or are other people in your department also unable to connect?"*

| Scope | Action |
|---|---|
| Isolated to one user | Continue to Step 3 — troubleshoot the workstation |
| Affecting multiple users or entire department | **Escalate immediately** — this is likely an infrastructure issue (switch, router, DHCP server). Do not troubleshoot at the workstation level. Document and escalate to Tier 2. |

---

## Step 3 — Determine Remote Access Availability

Ask yourself: **does the user have any network connectivity at all?**

- If the user has **partial connectivity** (network but no internet) — remote tools like RDP may still be available
- If the user has **zero network connectivity** — remote access is not possible. **You must go to the user's desk.**

> ⚠️ Do not attempt to remote into a machine with no network connection. Establish scope and connectivity status before choosing your approach.

---

## Step 4 — Physical Layer Check (Layer 1)

When at the user's desk or walking them through it over the phone:

- Check that the **ethernet cable is firmly plugged in** to both the machine and the wall/switch
- Check that the **NIC activity lights** are on (usually on the ethernet port itself)
- If on Wi-Fi — confirm the user is connected to the **correct network**
- Ask: *"Did you recently restart your computer, install updates, or make any changes?"*

---

## Step 5 — Run `ipconfig /all`

Open Command Prompt and run:

```
ipconfig /all
```

This single command tells you almost everything you need to know. Look at two things first:

1. **IP Address** — is it a valid IP or an APIPA address?
2. **DNS Servers** — what DNS server is the machine pointed to?

---

## Step 6 — Diagnose Based on IP Address

### Scenario A — APIPA Address (169.254.x.x)

An APIPA address means the machine **could not reach the DHCP server** and assigned itself a fallback address. The device is not properly on the network.

**Steps:**

```
ipconfig /release
ipconfig /renew
```

This forces the machine to redo the DHCP DORA process (Discover, Offer, Request, Acknowledge) and request a new IP address from the DHCP server.

| Result | Next Action |
|---|---|
| Valid IP assigned | Test connectivity — issue likely resolved |
| Still showing APIPA | DHCP server may be unreachable or the scope may be exhausted — escalate to Tier 2 |

---

### Scenario B — Valid IP Address

A valid IP address means **DHCP is working fine**. The problem is higher up — most likely DNS.

**Steps:**

Run the following two commands:

```
ping 8.8.8.8
ping google.com
```

| Result | What It Means |
|---|---|
| `ping 8.8.8.8` succeeds, `ping google.com` fails | Network connection is fine. DNS resolution is failing on this machine. |
| Both fail | The machine cannot reach anything outside the local network. Likely a gateway or infrastructure issue — escalate to Tier 2. |

---

## Step 7 — Resolving DNS Failure

If `ping 8.8.8.8` succeeds but `ping google.com` fails, DNS resolution is the problem.

**Step 7a — Flush the DNS Cache**

```
ipconfig /flushdns
```

This clears the local DNS cache on the machine. Test again after running this command.

| Result | Next Action |
|---|---|
| Issue resolved | Document and close the ticket |
| Issue persists | Continue to Step 7b |

---

**Step 7b — Check DNS Server Configuration**

Run `ipconfig /all` again and look at the **DNS Servers** field.

- In a corporate environment, the machine should be pointing to the **Domain Controller's IP address** (e.g., `192.168.10.1`)
- If it is pointing to an incorrect or unreachable DNS server, update the DNS server address in the network adapter settings to match the Domain Controller's IP

| DNS Server Field | Action |
|---|---|
| Pointing to wrong IP | Correct the DNS server to point to the Domain Controller |
| Pointing to correct IP but still failing | Issue is isolated to this machine and not resolvable at Tier 1 — escalate to Tier 2 with full documentation |

---

## Step 8 — Escalation

Escalate to Tier 2 in any of the following situations:

- Multiple users or entire department affected
- APIPA persists after release/renew (DHCP server issue)
- Both `ping 8.8.8.8` and `ping google.com` fail (gateway or infrastructure issue)
- DNS server is correct but resolution still fails after flushdns (machine-level issue beyond Tier 1 scope)

When escalating, document the following in the ticket:

- Steps already performed
- Output of `ipconfig /all`
- Results of ping tests
- Scope of the issue (isolated vs widespread)

---

## Full Troubleshooting Flow

```
Call received — user cannot connect to internet
        │
        ▼
Acknowledge user → Verify identity
        │
        ▼
Is it affecting others?
   ├── Yes → Escalate to Tier 2 immediately
   └── No  → Continue
        │
        ▼
Does user have any network connectivity?
   ├── No  → Go to user's desk
   └── Yes → Remote in if possible
        │
        ▼
Physical check — cable, NIC lights, correct network
        │
        ▼
Run: ipconfig /all
        │
        ▼
Check IP address
   ├── 169.254.x.x (APIPA) → ipconfig /release → ipconfig /renew
   │       ├── Fixed → Document and close
   │       └── Still APIPA → Escalate (DHCP issue)
   │
   └── Valid IP → ping 8.8.8.8 and ping google.com
           ├── Both fail → Escalate (gateway/infrastructure issue)
           └── 8.8.8.8 works, google.com fails → DNS issue
                   │
                   ▼
               ipconfig /flushdns
                   ├── Fixed → Document and close
                   └── Still failing → ipconfig /all → check DNS server
                           ├── Wrong DNS server → Correct to DC IP → test again
                           └── Correct DNS server but still failing → Escalate to Tier 2
```

---

## Key Commands Reference

| Command | Purpose |
|---|---|
| `ipconfig /all` | Full network adapter details including IP, DNS server, MAC address, DHCP status |
| `ipconfig /release` | Releases current IP address lease from DHCP server |
| `ipconfig /renew` | Requests a new IP address from DHCP server |
| `ipconfig /flushdns` | Clears the local DNS resolver cache |
| `ping 8.8.8.8` | Tests raw network connectivity bypassing DNS |
| `ping google.com` | Tests DNS resolution and internet connectivity together |

---

## Related Documentation
