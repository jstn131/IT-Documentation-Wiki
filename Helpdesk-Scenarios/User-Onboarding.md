# New Employee Onboarding — Full Ticket Walkthrough

## Overview
This document walks through a complete real world helpdesk ticket 
for a new employee onboarding scenario. It covers the full workflow 
from ticket submission to account creation using osTicket and 
Active Directory.

---

## Scenario
**User:** Ryan Smith — IT Department  
**Issue:** New employee needs domain account and system access created

---

## Step 1 — User Submits Ticket Via Customer Portal

<img width="2877" height="1550" alt="Screenshot 2026-04-15 234129" src="https://github.com/user-attachments/assets/6eee4fa8-47d7-4e27-a1b8-302dc75945eb" />

Ryan Smith submits a ticket through the osTicket customer portal 
reporting he is a new employee with no login credentials or system 
access.

> Note: In a real environment a new employee would not submit their 
> own onboarding ticket since they have no company account yet. 
> This would typically be submitted by HR or their manager before 
> their first day.

---

## Step 2 — Ticket Appears In Agent Queue

<img width="2879" height="1551" alt="Screenshot 2026-04-15 234154" src="https://github.com/user-attachments/assets/d8076b82-b9c5-445c-86cb-542905ab23eb" />

The ticket appears in the helpdesk queue. First steps:
- Assign ticket to yourself immediately
- Assess priority and SLA

---

## Step 3 — Assign SLA and Priority

<img width="2879" height="1551" alt="Screenshot 2026-04-15 234322" src="https://github.com/user-attachments/assets/c4fcdd64-5c01-45d1-8f2a-a0602cbcde85" />

**SLA assigned:** SEV-C Normal  
**Reasoning documented:** Standard new employee setup request. 
Not an emergency but should be resolved promptly so the employee 
can begin work.

---

## Step 4 — First Reply and Identity Verification

Before creating any account identity must be verified. A reply 
is sent asking for verification:

```
Hello Ryan, welcome to the company!
Before I create your account I need to verify
a few things for security purposes:

What is your employee ID?
What department will you be working in?
Who is your direct manager?

Once confirmed I will have your account ready shortly.
IT Helpdesk
```

> Never create an account without verifying identity first. 
> An unauthorized person could gain access to company systems 
> if credentials are created without proper verification.

---

## Step 5 — Navigate To Active Directory

After Ryan confirms his employee ID and manager we open 
Active Directory Users and Computers to create his account.

---

## Step 6 — Navigate To Correct OU

<img width="1027" height="770" alt="Screenshot 2026-04-15 235353" src="https://github.com/user-attachments/assets/ed59ff24-68e8-4fda-a982-ec15560e43f5" />

Ryan confirmed he is in the IT department. We navigate to 
the IT Organizational Unit where his account will live.

---

## Step 7 — Create New User Account

<img width="1025" height="760" alt="Screenshot 2026-04-15 235444" src="https://github.com/user-attachments/assets/cf2d73c5-8ec6-478a-9cc3-ffd11482bb78" />

Right click IT OU → New → User

Fill in the following:
- **First name:** Ryan
- **Last name:** Smith
- **Username:** ryan.smith
- **Format:** firstname.lastname — standard naming convention

---

## Step 8 — Set Password Settings

<img width="1023" height="763" alt="Screenshot 2026-04-15 235513" src="https://github.com/user-attachments/assets/17251357-0614-4de1-81a1-71b8e12b2f68" />

- Set a temporary password meeting complexity requirements
- Check **"User must change password at next logon"**
- This ensures Ryan creates his own private password on first login

---

## Step 9 — Add To Security Group

<img width="1026" height="767" alt="Screenshot 2026-04-15 235926" src="https://github.com/user-attachments/assets/ea3d6baa-5e05-46d0-9b3f-9411fdbd4159" />

Navigate to IT-Team Security Group → Properties → Members → Add

Add ryan.smith to the IT-Team Security Group.

This gives Ryan access to all IT department resources — shared 
drives, folders, and applications — automatically through group 
membership rather than individual permission assignment.

> This step is critical. Without Security Group membership Ryan 
> can log in but cannot access any department resources.

---

## Step 10 — Communicate Credentials Securely

**Username** is sent via ticket reply:

```
Hello Ryan,
Your account has been created successfully.
Your username is: ryan.smith
I will be calling you shortly to provide your
temporary password. Please have a pen ready to
write it down.
When you first log in you will be prompted to
create a new password. It must contain:

Uppercase letter
Lowercase letter
Number
Special character

IT Helpdesk
```

> Password is NEVER sent in the same message as the username. 
> Always communicate passwords via phone call only.

---

## Step 11 — Document In Internal Notes

```
INTERNAL NOTE:
New employee onboarding for Ryan Smith — IT Department.
Steps taken:

Identity verified via employee ID confirmation
User account created in IT OU
Username: ryan.smith
Temporary password set with forced change on first login
Added ryan.smith to IT-Team Security Group
Username sent via ticket reply
Temporary password communicated via phone call
User confirmed successful login

Resolution time: approximately 10 minutes
```

---

## Step 12 — Resolve and Close Ticket

Once Ryan confirms successful login and access to IT resources 
the ticket is marked as Resolved and closed.

---

## Key Takeaways

- Always verify identity before creating any account
- New employees should ideally have accounts created before 
  their first day — HR or manager submits the request
- Always add new users to the correct Security Group — 
  without it they cannot access department resources
- Never send username and password in the same message
- Always use firstname.lastname username format
- Always force password change on first login
- Document every step in internal notes
