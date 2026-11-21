# Hallway Monitor Setup — CHP West Valley Schedule

How to get the posted (Finalized) schedule onto the hallway monitors using a folder on the network share. No server or database is needed.

## How it works

1. The scheduling sergeant finalizes and posts the month in the schedule app as usual.
2. They click **📺 Publish for monitors** (on the Finalized Schedule tab, or under ⚙ Options → Hallway monitors).
3. The app writes six read-only pages into a folder on the network share:

   | File | Shows |
   |---|---|
   | `WVA-Monitor-A.html` | A Watch |
   | `WVA-Monitor-B.html` | B Watch |
   | `WVA-Monitor-C.html` | C Watch |
   | `WVA-Monitor-SD.html` | Special Duty |
   | `WVA-Monitor-SGT.html` | Sergeants |
   | `WVA-Monitor-Rotate.html` | All five sheets, switching every 20 seconds (for a hallway with only one screen) |

4. Each hallway computer has one of those files open full screen. The page **reloads itself every minute**, so a new publish appears within a minute with no one touching the monitor.

Things the pages handle on their own:

- **Month changeover:** each publish includes this month and next month (if next month has been posted). On the 1st, the monitors switch to the new month by themselves.
- **Next month not posted yet:** the monitor keeps showing the last posted month, with a red banner saying the new month hasn't been posted.
- **Fit to screen:** the sheet scales to fill the monitor at any resolution.
- **Read-only:** nothing on the monitor can be clicked or changed.
- A small footer shows when it was last published and by which S#.

---

## Part 1 — The folder (one time)

1. Create a folder on the network share, for example:
   `\\SERVER\Share\WVA-Schedule\Monitors`
2. Permissions:
   - **Sergeants who publish:** read **and write**.
   - **Hallway computers' login account:** read only is enough.

---

## Part 2 — Publishing (scheduling sergeant)

Use **Microsoft Edge** or **Google Chrome**. Firefox can't write to a folder; it downloads the six files instead, and you have to copy them into the share yourself.

**First time**

1. Open the schedule app, go to **⚙ Options → Hallway monitors**, and click **📺 Publish for monitors**.
2. A folder picker opens. Browse to the Monitors folder on the share (through *Network* or a mapped drive) and choose **Select folder**.
3. When the browser asks to let the site edit files in that folder, choose **Edit files** (or *Allow*).

**Every time after**

- Click **📺 Publish for monitors**. The app remembers the folder. The browser may ask for permission once per session; click *Allow*.
- When the Finalized Schedule has changed since the last publish (a sick day, a re-post, a roster change), the button turns gold with a **•**, and Options says **"Changed since — publish again."**
- **Publish after every change you want the hallway to see.** The monitors only show what was last published.

**If publishing fails** (share offline, no write access), the app offers to pick the folder again or download the files so you can copy them over by hand.

> Only the browser that holds the schedule data can publish. Right now that's the scheduling sergeant's computer: the app saves its data in that browser, not on the share.

---

## Part 3 — Hallway computers (department IT)

The monitors are just screens, so each one needs a small computer (a mini PC, thin client or old desktop) running a browser. It doesn't need special network routing. **If the computer can open the share, it can show the schedule.**

For each hallway computer:

1. **Network and login:** joined to the department network and signed in automatically as an account with read access to the Monitors folder.
2. **Start the schedule automatically at login.** Put a shortcut in the Startup folder (`shell:startup`) with a target like:

   ```
   "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --kiosk "file://SERVER/Share/WVA-Schedule/Monitors/WVA-Monitor-A.html" --edge-kiosk-type=fullscreen --no-first-run
   ```

   - Change `-A` to `-B`, `-C`, `-SD`, `-SGT` or `-Rotate` for each screen.
   - Use the server path (`file://SERVER/Share/...`) rather than a mapped drive letter. Mapped drives often aren't connected yet when the computer starts.
   - To get the address, you can open the file once in Edge from File Explorer and copy it from the address bar.
3. **Power:** set the screen and sleep to **Never**, and turn off the screen saver.
4. **Recovery:** if the share drops out while a page is reloading, the browser can get stuck on an error page. A **daily scheduled restart** of the computer (or of Edge), for example at 0300, clears this automatically. Edge's kiosk idle-reset settings are another option IT may prefer.

To exit kiosk mode on a hallway computer, press **Alt+F4**.

---

## Quick test

1. Publish from the app.
2. On any computer with access to the share, double-click `WVA-Monitor-A.html` in the Monitors folder. You should see A Watch filling the window.
3. Mark a change on the Finalized Schedule, publish again, and within about a minute the open page should update.
