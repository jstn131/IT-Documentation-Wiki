# First Response Checklists

## Overview
These checklists serve as quick reference guides for the most 
common helpdesk scenarios. Always work through these questions 
before making any changes in Active Directory or other systems.

---

## Account Lockout Checklist

### Before Touching Active Directory — Ask These First

**Identity Verification**
- [ ] What is your full name?
- [ ] What is your employee ID or department?
- [ ] What is your manager's name?
> Never make account changes without verifying identity first

**Basic Questions**
- [ ] Are you sure you are logging into the correct account?
      (Sometimes users log into a colleague's account by mistake)
- [ ] Are you logging into a domain account or local account?
      (Should see HOMELAB\username or username@homelab.local)
- [ ] Have you tried restarting your computer?
- [ ] Have you been away from work for an extended period?
      (Accounts can lock from cached credential attempts on return)
- [ ] Has anyone else used your computer recently?
      (Someone may have triggered the lockout on your machine)

> Note: Do NOT ask about Caps Lock for lockout scenarios.
> A locked account prevents login attempts entirely regardless 
> of what is typed. Caps Lock is only relevant for password issues.

**After Basic Questions Are Confirmed**
- [ ] Open Active Directory Users and Computers
- [ ] Ask which department they are in to locate their OU
- [ ] Find the user account
- [ ] Check Properties → Account tab
- [ ] Is Unlock Account checkbox active (ungreyed)?
      ├── Yes → account is locked → check the box → Apply
      └── No → account is not locked → proceed to password reset checklist

---

## Password Reset Checklist

### Before Resetting Anything — Ask These First

**Identity Verification**
- [ ] What is your full name?
- [ ] What is your employee ID or department?
- [ ] What is your manager's name?

**Basic Questions**
- [ ] Is Caps Lock turned off?
      (Caps Lock causes password failures — check this first)
- [ ] Are you sure you have forgotten your password?
      (Could be an account lockout instead — check AD first)
- [ ] Is your account locked or just password forgotten?
      (Check AD — if unlock checkbox is active it is a lockout not a reset)
- [ ] Are you logging into the correct account?
- [ ] Has your password recently expired?
- [ ] Have you recently changed your password on another device?
      (Cached credentials on this machine may be outdated)

**Password Reset Steps**
- [ ] Open Active Directory Users and Computers
- [ ] Navigate to user's department OU
- [ ] Right click user → Reset Password
- [ ] Set temporary password
- [ ] Check "User must change password at next logon"
- [ ] Check "Unlock account" checkbox if also locked
- [ ] Communicate temporary password via phone only
- [ ] Inform user of password complexity requirements:
      Uppercase, lowercase, number, special character required

---

## New Employee Onboarding Checklist

### Before Creating Anything — Ask These First

**Identity and Authorization Verification**
- [ ] What is your full name?
- [ ] What is your employee ID?
- [ ] What department will you be working in?
- [ ] Who is your direct manager?
- [ ] Has your manager or HR submitted confirmation of your hire?
> Never create an account based solely on the requestor's word
> Always verify through manager or HR before proceeding

**Information Needed To Create Account**
- [ ] Full legal name (for username format firstname.lastname)
- [ ] Department (determines which OU to place them in)
- [ ] Job role (determines which Security Group to add them to)
- [ ] Start date (account should be ready before first day)
- [ ] Any special access requirements beyond standard department access?

**Account Creation Steps**
- [ ] Open Active Directory Users and Computers
- [ ] Navigate to correct department OU
- [ ] Create new user — username format: firstname.lastname
- [ ] Set temporary password meeting complexity requirements
- [ ] Check "User must change password at next logon"
- [ ] Add user to correct Security Group for their department
- [ ] Verify account appears in correct OU
- [ ] Verify Security Group membership in Member Of tab

**Communicating Credentials**
- [ ] Send username via ticket reply or email
- [ ] Communicate temporary password via phone call ONLY
      Never send username and password in the same message
- [ ] Inform user of password complexity requirements:
      Uppercase, lowercase, number, special character
- [ ] Tell user to write down their new password somewhere safe
- [ ] Inform user that a password change prompt will appear on first login

**Confirmation Before Closing**
- [ ] Wait for user to confirm successful login
- [ ] Confirm they have access to department resources
- [ ] Ask if they need any additional access or software

---

## General First Response Rules

These apply to every single ticket regardless of issue type:

| Rule | Why It Matters |
|---|---|
| Always acknowledge the ticket first | User knows someone is helping them |
| Always verify identity before making changes | Prevents unauthorized account access |
| Always ask basic questions before touching AD | Prevents unnecessary changes |
| Always communicate passwords by phone only | Security — never in writing |
| Always wait for user confirmation before closing | Ensures issue is actually resolved |
| Always document steps taken in internal notes | Audit trail and knowledge base |
| Never delete accounts — disable only | 90 day policy, data preservation |
| Always assign ticket to yourself when working it | Prevents duplicate work |

---

## Quick Diagnostic Flow
```
User says they cannot log in
↓
Can they attempt to type a password?
├── No — screen immediately says account locked
│     → Use Account Lockout Checklist
└── Yes — they can type but password is rejected
→ Check Caps Lock first
→ Use Password Reset Checklist
→ May also be locked after too many failed attempts
```
