# Network Troubleshooting Guide

## Overview
This guide covers the standard troubleshooting process for users 
reporting internet connectivity issues. Follow each step in order 
before escalating to the next level.

---

## Common Scenario
User calls helpdesk reporting they cannot connect to the internet.

---

## Step 1 — Basic Checks (No Commands Needed)

Always exhaust basic troubleshooting before opening Command Prompt.
Most connectivity issues are resolved here.

| Check | How To Verify |
|---|---|
| **WiFi turned on?** | Check WiFi toggle in Settings or keyboard shortcut (Fn + WiFi key) |
| **Airplane mode off?** | Check Quick Settings in bottom right taskbar |
| **Connected to correct network?** | Sometimes devices connect to guest or neighbor networks with no internet |
| **Restart the device** | Clears temporary issues — always ask if they've tried this |
| **Ethernet cable secure?** | Check both ends — computer side AND router/switch side. Port light should blink |
| **Try a different browser** | If Chrome fails but Edge works — browser issue not network issue |
| **Is it one specific website?** | Check downdetector.com to see if the site is down for everyone |
| **Run Windows troubleshooter** | Settings → Network and Internet → Troubleshoot |
| **Toggle WiFi off and on** | Simple reset of the wireless adapter |
| **Forget and reconnect to network** | Clears cached credentials — forget network → reconnect → re-enter password |

If none of these resolve the issue move to Step 2.

---

## Step 2 — Network Reset

1. Go to **Settings → Network and Internet**
2. Click **Advanced Network Settings**
3. Click **Network Reset**
4. Restart the device when prompted

If still no connection after restart continue to Step 3.

---

## Step 3 — Check IP Address With ipconfig

Open **Command Prompt** and run:
```cmd
ipconfig
```

Look at the **IPv4 Address** field:

| IP Address Seen | What It Means | Next Step |
|---|---|---|
| `169.254.x.x` | APIPA — DHCP failed | Go to Step 4 |
| Valid IP `192.168.x.x` | Has IP, different issue | Go to Step 5 |
| Blank | No network adapter detected | Check hardware — Step 7 |

---

## Step 4 — APIPA Address Detected (169.254.x.x)

**What is APIPA?**
APIPA stands for Automatic Private IP Addressing. When a device 
cannot reach a DHCP server to obtain an IP address automatically 
Windows assigns itself a 169.254.x.x address as a fallback. 
This address cannot access the internet or any network resources.

**Fix — Release and Renew IP Address:**
```cmd
ipconfig /release
ipconfig /renew
```

**What this does:**
- `/release` — returns the current IP address back to the 
  DHCP server pool, ending the lease
- `/renew` — triggers the DORA process requesting a fresh 
  IP address from the DHCP server

**After running these commands run ipconfig again:**

| Result | Meaning | Action |
|---|---|---|
| Valid IP now showing | DHCP lease renewed successfully | Issue resolved ✅ |
| Still 169.254.x.x | DHCP server unreachable | Continue below |

**If still showing APIPA — check if other users are affected:**

| Scenario | Meaning | Action |
|---|---|---|
| Other users also affected | Network wide DHCP issue | Escalate to network team immediately |
| Only this user affected | Isolated device issue | Check physical connection |

**Possible causes of APIPA:**

| Cause | Description |
|---|---|
| DHCP server down | Server crashed or service stopped |
| DHCP scope exhaustion | No IP addresses left to assign — network team expands scope |
| Physical connection issue | Cable unplugged or WiFi disabled |

---

## Step 5 — Valid IP But Still No Internet

If the device has a valid IP address but still cannot access 
the internet the issue is likely connectivity or DNS related.

**Test basic internet connectivity:**
```cmd
ping 8.8.8.8
```

8.8.8.8 is Google's public DNS server. This tests whether your 
device can reach the internet at the network level — bypassing 
DNS entirely by using a direct IP address.

| Result | Meaning | Action |
|---|---|---|
| Gets replies | Internet connectivity working | DNS issue — go to Step 6 |
| Request timed out | Cannot reach internet at all | Routing issue — escalate |

**Understanding ping results:**
- **Reply** — destination is reachable
- **Request timed out** — cannot reach destination
- **ms value** — latency, how long packets take to travel
- **Packet loss** — percentage of data being dropped

---

## Step 6 — DNS Troubleshooting

**Why ping 8.8.8.8 can work but websites still fail:**

Browsing websites requires DNS to translate domain names into 
IP addresses. If DNS is broken your device can reach the internet 
by direct IP address but cannot resolve domain names.

```
ping 8.8.8.8     = calling someone using their phone number directly
ping google.com  = looking someone up by name in a phonebook
If the phonebook (DNS) is broken:
→ Direct number works (ping 8.8.8.8 succeeds)
→ Name lookup fails (ping google.com fails)
```

**Test DNS specifically:**
```cmd
ping google.com
```

| Scenario | Result | Action |
|---|---|---|
| ping 8.8.8.8 works, ping google.com fails | DNS broken | Run ipconfig /flushdns |
| ping google.com works but browser fails | Browser issue | Try different browser, clear cache |
| Both fail | No internet connectivity | Escalate |

**Fix — Flush DNS Cache:**
```cmd
ipconfig /flushdns
```

**Why this works:**
Your device caches DNS lookups locally to speed up connections. 
If a website moves to a new IP address your cache still holds 
the old address causing connections to fail. Flushing the cache 
forces your device to request fresh DNS information from the 
DNS server.

**If browser works but specific site fails:**
- Check downdetector.com — site may be down for everyone
- Try accessing the site on a different device
- If only this device fails — clear browser cache and cookies
- Disable browser extensions one by one to find the culprit
- Check browser proxy settings

---

## Step 7 — Hardware Check

If all software troubleshooting has failed consider physical 
hardware issues:

**WiFi:**
- Is the WiFi card properly seated in its slot?
- Are antenna cables connected to correct ports?
- MAIN and AUX antenna cables must not be swapped or disconnected
- Try connecting via ethernet cable instead to isolate WiFi issue

**Ethernet:**
- Try a different ethernet cable
- Try a different port on the switch or router
- Check if the NIC shows in Device Manager without errors
- Port light on the switch should blink when cable is connected

---

## Step 8 — Escalate

If none of the above steps resolve the issue escalate to 
Level 2 or the network team with detailed notes including:

- All steps already attempted
- Results of each command run
- Whether other users are affected
- Any error messages seen

> **Never escalate without documenting what you already tried.**
> This prevents Level 2 from repeating your steps and speeds 
> up resolution time significantly.

---

## Complete Troubleshooting Flow

| Step | Action | If Resolved | If Not Resolved |
|---|---|---|---|
| 1 | Basic checks — WiFi, airplane mode, restart, cable | Close ticket | Continue |
| 2 | Network reset | Close ticket | Continue |
| 3 | ipconfig — check IP address | — | Check result |
| 4 | APIPA detected — release/renew | Close ticket | Escalate if persists |
| 5 | ping 8.8.8.8 — test connectivity | Continue to DNS | Escalate |
| 6 | ping google.com — test DNS, flushdns | Close ticket | Try browser fixes |
| 7 | Hardware check — WiFi card, cables, NIC | Close ticket | Escalate |
| 8 | Escalate with detailed notes | — | — |

---

## Command Reference

| Command | Purpose |
|---|---|
| `ipconfig` | View current IP address and network configuration |
| `ipconfig /release` | Return IP address to DHCP pool |
| `ipconfig /renew` | Request new IP address from DHCP server via DORA process |
| `ipconfig /flushdns` | Clear local DNS cache |
| `ping [address]` | Test connectivity and measure latency |

---

## Key Terms

| Term | Definition |
|---|---|
| **APIPA** | 169.254.x.x — automatically assigned when DHCP server is unreachable |
| **DHCP** | Service that automatically assigns IP addresses to devices on a network |
| **DNS** | Translates domain names like google.com into IP addresses |
| **Latency** | Time in milliseconds for a packet to travel to destination and back |
| **DHCP Scope** | Range of IP addresses available for the DHCP server to assign |
| **Scope Exhaustion** | All IP addresses in the DHCP scope are currently leased out |
| **DNS Cache** | Local storage of recent DNS lookups to speed up future connections |
| **DORA** | Discover, Offer, Request, Acknowledge — the DHCP IP assignment process |
| **Packet Loss** | Percentage of data packets that fail to reach their destination |
