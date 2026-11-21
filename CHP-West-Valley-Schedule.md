# CHP West Valley Area — Officer & Sergeant Schedule

A single-file web app (`chp-schedule.html`) for building, checking, posting and displaying the monthly duty schedule for the CHP West Valley Area. It reproduces the CHP 231A schedule form layout for each watch and adds RDO bidding, special-request approval, FLSA checks, day rosters, printing and hallway-monitor display.

- **Type:** one self-contained HTML file (HTML + CSS + vanilla JS, no build step, no external dependencies; the CHP logo is embedded)
- **Runs in:** any modern browser (Edge / Chrome recommended for printing)
- **Data:** stored in the browser's `localStorage` (see [Data & storage](#data--storage))
- **Data version:** v3 (`chp-wva-schedule-v3`); backups from the earlier app load directly

---

## Who uses it

| Role | What they do |
|---|---|
| **Scheduling sergeant** | Maintains the roster and shift setup, reviews RDO submissions, approves/denies special requests, builds the Working Schedule, finalizes and posts the month. |
| **Any sergeant** | Marks sick days, vacation and other changes on the posted Finalized Schedule, stamped with their S#. |
| **Officers** | Open a personal RDO link to pick RDOs (X), vacation (V) and special requests for the coming month. |
| **Hallway monitors** | Display the Finalized Schedule full screen, auto-refreshing every minute. |

---

## Navigation

The top bar holds the brand, a month navigator (◀ / ▶), the main tabs, an acting-sergeant (S#) picker, and Undo / Redo.

| Tab / route | Purpose |
|---|---|
| `#requests` — **Officer Requests** | What each officer submitted for the month, who hasn't submitted, and pending special requests (badge shows the pending count). |
| `#working` — **Working Schedule** | The scheduling sergeant's draft. Submissions are already applied. |
| `#final` — **Finalized Schedule** | The posted schedule — what prints and what the monitors show. |
| `#roster` — **Roster** | People, badges, ranks, assignments, cars, shift lengths, emails, RDO links. |
| `#setup` — **Shift setup** | Watch hours/units/priority, MEP/HEP tags, FLSA rules, request settings, specialties, range & training days. |
| `#options` — **⚙ Options** | Backup/restore, CSV export, safety copies, hallway-monitor links, reset. |
| `#/rdo/<id>[/<YYYY-MM>][/solo]` | An officer's personal RDO page. `/solo` hides all sergeant controls (this is the link officers get). |
| `#kiosk/<A\|B\|C\|SD\|SGT>` | Full-screen hallway-monitor view of one sheet. |

Each schedule tab has a collapsible **More info** panel explaining how that screen works.

---

## Schedule sheets

Five sheets, each rendered on the CHP 231A form design:

| Sheet | Who appears |
|---|---|
| **A Watch** | A Watch, plus SSP, TRACE and Follow-Up |
| **B Watch** | B Watch, plus Malibu |
| **C Watch** | C Watch (officers grouped by car number / partner pairs; "Solo" for single-officer cars) |
| **Special Duty** | SD |
| **Sergeants** | Everyone with rank Sergeant |

Each sheet shows the days of the month (weekends shaded), a **PRIORITY** row with the minimum staffing per day (editable; days below priority show red), MEP/HEP tag bars printed across date ranges, and a legend.

### Default watches

| Watch | Hours | Units | Default priority |
|---|---|---|---|
| A | 0500–1430 | 21 | 8 |
| B | 1245–2215 | 20 | 10 |
| C | 2030–0600 | 10 (cars) | 6 |
| Special Duty | 0800–1700 | 8 | 2 |

All editable under **Shift setup**.

---

## Marking the schedule

Pick a code from the **Mark** bar (or press its key), then click or drag across cells. Clicking a cell that already has that code clears it. Right-click a cell for every code or up to 5 characters of custom text.

### Quick keys

| Key | Code |
|---|---|
| `X` | X — RDO |
| `U` | <u>X</u> — special-request RDO (underlined) |
| `V` | V — Vacation |
| `S` | S — Self sick |
| `F` | FS — Family sick |
| `T` | T — Area Training Day |
| `0` / Delete / Backspace | Erase |
| `Ctrl+Z` / `Ctrl+Y` (or `Ctrl+Shift+Z`) | Undo / Redo (60 steps) |
| `Esc` | Close dialog / exit full screen |

TVL, SRT and RAM also have quick buttons. On the Officer Requests tab only X and V are available.

### All codes

X (RDO), <u>X</u> (special-request RDO), V (Vacation), S (Self sick), FS (Family sick), T (Area Training Day), AT (Academy Training), TVL (Travel), SRT (Street Racing Training), PSD (PSD assignment), FMLA, IDL (Injury Disability Leave), ML (Military Leave), SD (Special Duty), FT (FTEP Day), RAM / RPM (Range), MT (Motor Training), OST (Office Safety Training), OIC (Officer In Charge Training), BPU (Bike Patrol), CPS (Car Seats), CBR (County Breath Training), BR (City/County Breath Training), AR (ARIDE), MV (MVARS), PS (Peer Support), EX (Explorers).

Sergeant sheet codes: S, FS, X, <u>X</u>, V, SRT, PSD.

### Date actions

- **Click a date number** → that day's roster in running-priority order (printable / PDF, can open in a new tab; includes "working units" sheets for A/B/C and specialty badges).
- **Right-click a date** → add MEP / HEP tags and toggle range (R) or training (T) days.

---

## Monthly workflow

1. **Bid window opens.** Officers get their personal RDO links (Roster → **✉ Email officer links**).
2. **Officers submit.** On their RDO page they pick RDO (X), Vacation (V) or Special request days on a calendar. A live meter shows RDOs picked vs. the month's target, and an FLSA check runs for every 28-day period that overlaps the month.
3. **Submissions auto-apply** to the Working Schedule — no approval needed. If a day already has another code, it's left alone and flagged as a conflict (red outline on the Requests grid).
4. **Special requests** are the only thing needing approval. Sergeants Approve / Deny / Reopen from the Officer Requests tab. Approved days are written as <u>X</u> (on Working and, if already posted, on Finalized) unless another non-RDO code is already there.
5. **Chase missing bids.** The Requests tab lists officers who haven't turned in RDOs, with **✉ Email reminder to these officers**.
6. **Build the draft** on the Working Schedule. The **Staffing check** card lists days below priority for A, B and C watches.
7. **Finalize & post.** Copies Working → Finalized and logs who posted it and when. On a re-post you choose to keep changes made directly on Finalized (sick days, etc.) or **Replace everything**.
8. **Day-to-day changes** go on the Finalized Schedule. Every change made after posting (sick call, discretionary vacation, erasing a day, any code) requires an S# (asked once per session). The cell gets a **red outline** and the sergeant's **S# in red in the bottom-right corner**. Setting a cell back to exactly what was posted removes both. The red marks show only on the Finalized Schedule, its printouts and the hallway monitors, never on Officer Requests or the Working Schedule. Re-posting with **Replace everything** starts clean; **Keep** keeps the outlined changes.

### Bid deadline

- Default deadline: day **20** of the prior month at 2359 (configurable, with a per-month override date).
- Optional **lock submissions after the deadline**; otherwise late submissions are accepted and marked late.
- A month can be **reopened** after the deadline.
- Sergeants can delete an officer's submission; only the cells that submission placed on Working are removed.

---

## FLSA rules

RDO requirements are checked per **28-day FLSA period**.

| Setting | Default |
|---|---|
| Period epoch (a start date) | 2026-10-05 (10/05/2026 – 11/01/2026) |
| Hour limit per period | 171 |
| Premium per shift | 0.5 h |
| 12-hour short-shift length | 7.5 h |

| Shift length | RDOs per period (default) |
|---|---|
| 8-hour | 8 |
| 9-hour (default shift) | 10 |
| 10-hour | 12 |
| 12-hour | 14 |

Individual officers can have an RDO override in their profile. The officer page reports each overlapping period as **OK**, **SHORT**, **EXTRA**, or **OVER 171** (over the hour cap — blocks submission). For periods that run past the month it reports whether the officer is on pace, light or heavy.

---

## Roster

- Table of everyone (sergeants highlighted) with inline-editable badge, rank, assignment, C-Watch car, seat, shift length, work email, and RDO link.
- **Assignments:** A Watch, B Watch, C Watch, SSP, TRACE, Follow-Up, Malibu, Special Duty, Admin.
- **Profile** (click a name): work email, specialties, RDO override, email the officer, remove.
- **Specialties** (shown on day rosters, not on the schedule): SRT, FTO, MTR, K9, MAIT, DRE, CVE, BIL, AIR, DIVE, HG, PS — editable in Shift setup.
- **Paste from Excel:** paste rows with columns `Badge, Name, Rank, Assignment, Shift, Email, Car` (tab- or comma-separated). Matches existing people by badge to update; adds the rest.
- A, B and Special Duty sort by badge/ID, which is also running-priority order on the day roster.

---

## Email

The app never sends mail itself. **Open in Outlook** builds a `mailto:` link that opens a draft in the default mail program. Long recipient lists are split into batches of 25 (BCC). If the link is blocked, the recipient, subject and body can each be copied and pasted.

Used for: officer RDO links, missing-bid reminders, emailing an individual officer, and special-request notices to the scheduling sergeant's email (set in Shift setup).

---

## Printing & display

- **Print** (current sheet) and **Print all** (all five sheets). Turn on *Background graphics* in the print dialog so colors print.
- **Day roster:** Print / Save as PDF, plus working-units sheets for A/B/C.
- **📺 Publish for monitors** (Finalized tab, or Options → Hallway monitors): writes read-only pages for each sheet (`WVA-Monitor-A/B/C/SD/SGT.html`, plus `WVA-Monitor-Rotate.html`, which cycles through all five) into a folder on the network share. Each page reloads every minute, scales to fit the screen, carries this month and next (posted months only), and switches months on the 1st by itself. The folder is remembered in the browser (Edge/Chrome; Firefox downloads the files instead). The button turns gold with a • when the Finalized data has changed since the last publish. Full setup: [Hallway-Monitor-Setup.md](Hallway-Monitor-Setup.md).
- **Live kiosk preview:** open the app at `#kiosk/A` (or B, C, SD, SGT). Shows the current month's Finalized Schedule from *this browser's* data (falls back to Working if not posted) and refreshes every 60 seconds. `Esc` or **Exit full screen** returns to the app.
- **Export to Excel (CSV):** the current month, every person — Finalized if posted, otherwise Working.

---

## Data & storage

All data lives in the browser:

| Key | Storage | Contents |
|---|---|---|
| `chp-wva-schedule-v3` | localStorage | Entire app state |
| `chp-wva-snapshots-v3` | localStorage | Up to 12 automatic safety copies |
| `chp-wva-session` | sessionStorage | Acting sergeant (S#) for this tab |

**State shape (top level):** `people`, `marks` (Working), `finalMarks` (Finalized), `postedMarks` (copy of each month as posted, used to detect later changes), `finalized` / `posted` (per month), `watches`, `priority`, `tags` (MEP/HEP), `dayFlags` (R/T days), `requests` (special requests), `rdoSubs` (RDO submissions per month), `rdoWindows` (deadline overrides / reopen), `specialties`, FLSA settings (`shiftRdos`, `shiftPremium`, `shortShift12`, `flsaCap`, `flsaEpoch`), `deadlineDay`, `lockAfterDeadline`, `requestEmail`, `log` (posting history per month). Marks are keyed `personId|YYYY-MM-DD`.

**Safety nets**

- Automatic safety copy on the first open each day and before every restore, load or reset (Options → Restore).
- Undo/redo history of 60 steps.
- **Save file** downloads a full `.json` backup; **Load file** restores one (current or earlier-app format).
- Multiple tabs in the same browser stay in sync via the `storage` event.

> **Limitation:** until a shared backend is connected, data exists only in the browser where it was entered — an officer submitting from their own computer won't appear on the sergeant's copy. All storage goes through the `Store` object, the single place to swap in a shared backend (e.g. SharePoint / Dataverse).

---

## Code layout (inside the `<script>`)

| Area | Key functions / constants |
|---|---|
| Storage | `Store`, `persist`, `snapshot`, `commit`, `undo`, `redo` |
| Constants | `MONTHS`, `ASSIGNMENTS`, `DEFAULT_WATCHES`, `CODES`, `QUICK`, `SGT_CODES`, `DEFAULT_PEOPLE`, `DEFAULT_SPECIALTIES` |
| State | `freshState`, `normalize`, `loadFileData` |
| Sheets | `SHEETS`, `FORM`, `renderFormA/B/C/SD/Sgt`, `formsHtml`, `sheetForPerson` |
| RDO bids | `applySub`, `deleteSub`, `rdoAnalysis`, `deadlineFor`, `pastDeadline`, `locked` |
| FLSA | `overlappingFlsa`, `requiredRdos`, `hoursForWorkDays` |
| Special requests | `decideRequest`, `undoApplied`, `pendingRequests` |
| Posting | `postMonth`, `finalizedOnlyChanges`, `logIt` |
| Change marks | `isChanged`, `postedCode`, `showChanges` (red outline + S# on Finalized only) |
| Routing / render | `route`, `go`, `render`, `renderBar`, `pgSchedule`, `pgRoster`, `pgSetup`, `pgOptions`, `pgRdo`, `pgKiosk` |
| Email | `mailtoHref`, `emailDraft`, `remindMissing`, `emailLinks` |
| Output | `printSheets`, `openDay`, `exportCsv`, `saveFile` |
| Monitors | `publishMonitors`, `monPageHtml`, `monMonths`, `monSig` / `monStale`, `MonDB` (remembers the folder in IndexedDB), `MON_FILES` |

---

## Possible next steps

- Connect `Store` to a shared backend so officer submissions reach the scheduling sergeant automatically.
- Real authentication instead of the honor-system S# picker and shareable RDO links.
- Server-side email notifications for special requests and deadline reminders.
