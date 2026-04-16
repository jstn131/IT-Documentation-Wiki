# Trust Relationship Error — Troubleshooting Guide

## Overview
The trust relationship error is one of the more serious domain 
connectivity errors a helpdesk technician will encounter. This 
guide explains what it is, why it happens, and how to respond.

---

## What Is The Trust Relationship Error?

### Technical Definition
The trust relationship error occurs when the machine account 
password stored on the workstation and the matching record on 
the Domain Controller fall out of sync. When this happens the 
workstation and Domain Controller no longer recognize each other 
and authentication fails completely.

Every computer joined to a domain has its own machine account 
in Active Directory — just like users have user accounts. This 
machine account has an automatically generated password that 
rotates every 30 days. Both the workstation and Domain Controller 
store a copy of this password. When both copies match the trust 
relationship is healthy. When they fall out of sync the trust 
relationship fails.

### Simple Definition
The trust relationship error is when your computer and the 
Domain Controller no longer have the same password. Since they 
no longer share the same information they fall apart and can 
no longer communicate or trust each other.

### Analogy — For Non Technical Users
Think of it like a relationship. The computer is the boyfriend 
and the Domain Controller is the girlfriend. They had a shared 
understanding between them — their passwords matched and they 
trusted each other completely.

Something happened that caused one of them to change without 
telling the other. Now they are out of sync, they no longer 
trust each other, and the relationship broke down. IT has to 
step in and get them back on the same page.

---

## What Is A Machine Account Password?

When a computer joins a domain Windows automatically:
- Creates a computer account in Active Directory
- Generates a random machine account password
- Stores it on both the workstation and the Domain Controller
- Rotates it automatically every 30 days

This password is never seen or typed by anyone — it works 
completely invisibly in the background. You only notice it 
when something goes wrong and it falls out of sync.

---

## What Happens During Login

Most people don't realize there are actually two authentications 
happening every time someone logs into a domain joined computer:

```
User sits down and types their password
↓
Step 1 — Computer authenticates to DC (invisible)
Computer presents its machine account password
DC verifies it matches its records
Trust confirmed ✅
↓
Step 2 — User authenticates to DC (visible)
User credentials verified against NTDS.dit
Access granted ✅
```
The trust relationship error breaks Step 1 — the computer 
cannot even get to the user authentication stage.

---

## Common Causes

| Cause | What Happened |
|---|---|
| **VM snapshot restored** | Workstation reverted to old password. DC already updated to new password. Mismatch. |
| **Computer offline too long** | Workstation missed the 30 day automatic rotation. DC updated, workstation did not. |
| **Computer account deleted in AD** | DC has no record of the machine at all. |
| **Domain Controller issue** | DC had a problem causing sync to fail across multiple machines. |

> **Important:** If multiple computers show this error simultaneously 
> it is almost certainly a server side issue — not individual machine 
> problems. Escalate immediately.

---

## How To Explain It At Different Levels

### To A Non Technical User
*"Your computer and the server that manages it no longer 
recognize each other because their shared connection got 
broken. You won't be able to log in until IT fixes it. 
We are working on it now."*

### To A Helpdesk Interviewer
*"The trust relationship error means the machine account 
password between the workstation and Domain Controller fell 
out of sync. Common causes include VM snapshot rollbacks 
which revert the workstation to an older password the DC 
no longer recognizes, or extended offline periods where 
the workstation misses the automatic 30 day password 
rotation cycle."*

### To A Senior Sysadmin When Escalating
*"CLIENT01 is throwing a trust relationship error. Checked 
AD — computer account still exists in the Computers container. 
Likely a password sync issue from a snapshot restore or 
extended offline period. Needs Reset-ComputerMachinePassword 
or domain rejoin."*

---

## Level 1 Response Procedure

```
1. Acknowledge ticket as Emergency if multiple users affected

2. Gather information immediately:
   - How many computers are affected?
   - Did this start suddenly or gradually?
   - Were any changes made recently?
   - Can users log in with a local account?

3. Check Active Directory:
   → Open AD Users and Computers
   → Check Computers container
   → Are the affected computer accounts still there?
   ├── Missing → computer accounts deleted → tell Level 2
   └── Present → password sync issue → escalate with details

4. Keep users updated every 15-20 minutes
   Never leave users in silence during an emergency

5. Escalate to Level 2 with all gathered information

6. Document everything in the ticket
```
---

## The Fix — Level 2 and Above

> Level 1 does not perform these fixes. This is for awareness only.

**Method 1 — Rejoin The Domain**
- Log in with local Administrator account
- Unjoin from domain → restart
- Rejoin domain → restart
- New machine account and password established
- Trust restored ✅

**Method 2 — PowerShell Reset (Faster)**
```powershell
Reset-ComputerMachinePassword -Server "WIN-1FGEG1FMJLD"
```
Resets the machine account password on both sides simultaneously 
without requiring a domain rejoin. Preferred for single machines.

**For multiple machines — scripted fix:**
A senior sysadmin would run the PowerShell command across all 
affected machines simultaneously using a script. This is why 
mass trust relationship errors are Level 2 and above.

---

## Error Message Reference

```
"The trust relationship between this workstation 
and the primary domain failed."
```

This error appears at the Windows login screen when a domain 
joined computer attempts to authenticate and the machine account 
password is out of sync with the Domain Controller.

---

## Key Facts For CompTIA A+

| Fact | Detail |
|---|---|
| **Root cause** | Machine account password out of sync |
| **Rotation frequency** | Every 30 days automatically |
| **Who sets the password** | Windows — automatically generated |
| **Common cause** | VM snapshot restore or extended offline period |
| **Level 1 action** | Gather info, check AD, escalate |
| **Level 2 fix** | Domain rejoin or Reset-ComputerMachinePassword |
