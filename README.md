# 👻 PPPoE Ghost Session Killer (Automated Fix)

![Platform](https://img.shields.io/badge/Platform-MikroTik%20RouterOS-red)
![Function](https://img.shields.io/badge/Function-Session%20Management-blue)
![Fix](https://img.shields.io/badge/Fix-Simultaneous%20Login%20Error-green)

## 🆘 The Problem
In ISP networks, unexpected power outages or router restarts at the client side often leave a **"Ghost Session"** on the NAS (MikroTik).
- The NAS thinks the user is still online.
- When the user tries to reconnect, they get the error: **"simultaneous login limit reached"**.
- Support staff has to manually find and remove the active connection.

## 🛠️ The Solution
This repository provides a **Scheduled Script** that acts as a Watchdog. It scans all active PPPoE connections and identifies "Stale" or "Ghost" sessions based on traffic inactivity and clears them automatically.

### ⚙️ How Logic Works
1.  **Iterate:** Loops through all active `<pppoe-connection>`.
2.  **Inspect:** Checks if the connection has been up for more than 10 minutes but has transferred **< 1KB** of data (Typical sign of a ghost).
3.  **Action:** Removes only the specific hung connection.
4.  **Log:** Records the incident for the admin.

## 🚀 Installation Guide

### Step 1: Upload the Script
Copy the code from `ghost_buster.rsc` and paste it into a new script named `kill_ghosts` in **System > Scripts**.

### Step 2: Schedule it
To run this check every 5 minutes:
```bash
/system scheduler add name="GhostCheck" on-event="kill_ghosts" interval=5m

Step 3: Manual Usage (For Helpdesk)
If a specific user is stuck, you can run:
:global UserToKill "client_username"
/system script run kill_specific_user

Author: Sheikh Alamin Santo
Network Reliability Engineer (NRE)
