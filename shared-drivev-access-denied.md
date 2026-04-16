# Shared Drive Access Denied — Troubleshooting Guide

## Overview
Access Denied errors on shared network drives are one of the most 
common helpdesk tickets. This guide covers the standard 
troubleshooting procedure from first response to resolution.

---

## Common Scenario
User reports they cannot access a shared network drive they 
previously had access to, receiving an "Access Denied" error.

---

## Most Common Causes

| Cause | Likelihood |
|---|---|
| Removed from Security Group | Most common |
| Cached credential issue after password reset | Common |
| Network drive mapping broken | Common |
| User profile corruption | Less common |
| Server side permission issue | Less common |

---

## Step 1 — Acknowledge and Gather Information

Reply to the user promptly and ask:

- Are you logged into your domain account or local account?
- Are you connected to the network?
- What is the exact error message you are seeing?
- Did anything change recently — password reset, new computer, 
  account changes?
- **Is anyone else in your department experiencing the same issue?**

> The last question is critical. If multiple users are affected 
> it is a server or Group Policy issue — not a user specific 
> problem — and should be escalated immediately.

---

## Step 2 — Check Security Group Membership In AD

Open **Active Directory Users and Computers** and locate the user.

**Fastest method — Check from the user account directly:**
1. Right click user → **Properties**
2. Click **Member Of** tab
3. Review all groups the user belongs to

> The Member Of tab shows every Security Group the user belongs 
> to in one place — faster than checking each group individually.

**What to look for:**

| Result | Meaning | Action |
|---|---|---|
| Correct Security Group listed | Membership is fine | Continue to Step 3 |
| Security Group missing | User lost group membership | Add back to group → test |

**If Security Group was missing:**
- Add user back to the correct Security Group
- Ask user to log out and log back in
- Confirm they can now access the shared drive
- Document what group was missing and when it was removed
- Close ticket if resolved ✅

---

## Step 3 — Rule Out Session and Credential Issues

If Security Group membership is correct try these steps:

**Ask the user to:**
1. Lock their workstation (Windows + L) and unlock it
2. If still failing — log out completely and log back in
3. Try accessing the shared drive again

**Why this works:**
Sometimes after a password reset or account change the user's 
active session still holds old cached credentials. A fresh 
login forces Windows to pull updated permissions from the 
Domain Controller.

---

## Step 4 — Test From A Different Machine

Ask the user to log into a different domain joined computer 
and try accessing the shared drive from there.

| Result | Meaning | Action |
|---|---|---|
| Works on different machine | Profile or workstation issue | Investigate user profile |
| Fails on different machine too | Account or server issue | Continue investigating |

---

## Step 5 — Check If Others Are Affected

If not already asked — confirm whether anyone else in the 
same department has the same issue.

| Scenario | Meaning | Action |
|---|---|---|
| Only this user affected | Isolated account issue | Continue troubleshooting user account |
| Multiple users affected | Server or GPO issue | Escalate to senior admin immediately |

---

## Step 6 — Remap The Network Drive

Sometimes the shared drive mapping itself becomes broken 
even if permissions are correct.

**Ask the user to:**
1. Open File Explorer
2. Right click **This PC** → **Map Network Drive**
3. Enter the shared drive path — example: `\\SERVER\Finance`
4. Check **Reconnect at sign-in**
5. Click Finish and test access

---

## Step 7 — Escalate

If none of the above steps resolve the issue escalate to 
Level 2 with detailed notes including:

- Whether others are affected
- Security Group membership status
- Steps already attempted
- Results of each step
- Exact error message

> Never escalate without documenting everything you tried. 
> This prevents Level 2 from repeating your steps.

---

## Complete Troubleshooting Flow

| Step | Action | If Resolved | If Not |
|---|---|---|---|
| 1 | Gather information — ask key questions | — | Continue |
| 2 | Check Member Of tab in AD | Close ticket ✅ | Continue |
| 3 | Log out and log back in | Close ticket ✅ | Continue |
| 4 | Test from different machine | Investigate profile | Continue |
| 5 | Check if others affected | — | Escalate if yes |
| 6 | Remap network drive | Close ticket ✅ | Continue |
| 7 | Escalate with detailed notes | — | — |

---

## Key Notes

- Always check the **Member Of tab** first — it is the fastest 
  way to identify Security Group issues
- A fresh login resolves most cached credential issues
- If multiple users are affected escalate immediately — 
  do not spend time troubleshooting individual accounts
- Always confirm with the user before closing the ticket
