# Account Lockout — Full Ticket Walkthrough

## Overview
This document walks through a complete real world helpdesk ticket 
for an account lockout scenario. It covers the full workflow from 
ticket submission to resolution using osTicket and Active Directory.

---

## Scenario
**User:** John Smith — Finance Department  
**Issue:** Account locked out, cannot log in, has an important deadline

---

## Step 1 — User Submits Ticket Via Customer Portal

<img width="2879" height="1550" alt="Screenshot 2026-04-15 231025" src="https://github.com/user-attachments/assets/e2956c09-4e5d-4e10-bd35-afbd7693fdfb" />

John Smith submits a ticket through the osTicket customer portal 
reporting he cannot log into his computer. He mentions having an 
important deadline making this time sensitive.

---

## Step 2 — Ticket Appears In Agent Queue

<img width="2879" height="1557" alt="Screenshot 2026-04-15 231049" src="https://github.com/user-attachments/assets/33bf3d7c-85aa-49dc-98b1-420b09a9a167" />

The ticket appears in the helpdesk queue unassigned. As the 
responding agent the first step is to:
- Assign the ticket to yourself to prevent duplicate work
- Assess the correct priority level and SLA

---

## Step 3 — Assign SLA and Priority

<img width="2879" height="1555" alt="Screenshot 2026-04-15 231437" src="https://github.com/user-attachments/assets/0ec043ba-c3dc-4589-8983-48f0b2fa9a19" />

**SLA assigned:** SEV-B High  
**Reasoning documented:** John Smith has been unable to log in 
for an extended period and has an important deadline. This requires 
urgent resolution.

Setting the correct SLA ensures the ticket is flagged appropriately 
and resolved within the required timeframe.

---

## Step 4 — First Reply To User

Before touching Active Directory a reply is sent to John to:
- Acknowledge the ticket warmly
- Verify his identity
- Ask basic troubleshooting questions
  
```
Hello John,
I have received your ticket and will resolve this
as quickly as possible.
Before proceeding I have a few questions:

What is your username or employee ID?
Are you logging into the correct account?
Have you tried restarting your device?

Please get back to me and I will resolve this immediately.
IT Helpdesk
```

---

## Step 5 — Navigate To Active Directory

After confirming basic questions John confirms everything is 
correct on his end. We now open Active Directory Users and 
Computers to investigate his account.

---

## Step 6 — Locate User In Correct OU

John confirmed he is in the Finance department. We navigate to 
the Finance OU and locate John Smith's account.

---

## Step 7 — Check Account Status

<img width="1019" height="761" alt="Screenshot 2026-04-15 232642" src="https://github.com/user-attachments/assets/8373c615-882c-489e-9e18-49340a98ad20" />

Right click John Smith → Properties → Account tab

The **Unlock Account checkbox is active (ungreyed)** confirming 
his account is genuinely locked. This means too many failed login 
attempts triggered the domain lockout policy.

---

## Step 8 — Unlock The Account

Check the Unlock Account checkbox and click Apply then OK.

John's account is immediately unlocked. He can now log in with 
his existing password — no password reset required.

---

## Step 9 — Notify User Of Resolution

A reply is sent to John informing him his account has been unlocked:

```
Hello John,
Your account has been successfully unlocked. Please
attempt to log in now and confirm you have full access.
If you experience any further issues please don't
hesitate to reply to this ticket.
Best regards,
IT Helpdesk
```

---

## Step 10 — Document In Internal Notes

An internal note is added documenting everything for the audit trail:

```
INTERNAL NOTE:
Account lockout confirmed in Active Directory — Finance OU.
Unlock Account checkbox was active indicating genuine lockout.
Account unlocked at [time].
User confirmed successful login via phone.
Root cause: Multiple failed login attempts.
Resolution time: approximately 10 minutes.
```

---

## Step 11 — Resolve and Close Ticket

Once John confirms successful login the ticket is marked 
as Resolved and closed.

---

## Key Takeaways

- Always verify identity before making any AD changes
- Check the Unlock Account checkbox — if active the account is locked
- Caps Lock is irrelevant for lockout scenarios — locked accounts 
  reject all login attempts regardless of what is typed
- Always document steps taken before closing
- Never close a ticket until the user confirms resolution
