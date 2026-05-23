# Protocol Tracker — Setup

## Files in this bundle
- `index.html` — the entire app (UI, logic, styles)
- `sw.js` — service worker for offline support

Both files must sit in the **same folder** at the repo root.

---

## 1. Create GitHub repo

1. Go to github.com → New repository.
2. Name it `protocol` (or anything — but if you don't name it `<username>.github.io`, the URL will include the repo name).
3. Set it to **Public** (required for free GitHub Pages).
4. Don't initialise with a README. Click Create.

## 2. Upload the files

1. On the empty repo page, click **uploading an existing file**.
2. Drag both `index.html` and `sw.js` into the upload zone.
3. Commit directly to `main`.

## 3. Enable GitHub Pages

1. Repo → **Settings** → **Pages** (left sidebar).
2. Source: **Deploy from a branch**.
3. Branch: **main**, folder: **/ (root)**. Save.
4. Wait ~60 seconds. A green banner appears with your URL:
   `https://<username>.github.io/protocol/`

## 4. Add to iPhone home screen

1. Open Safari on iPhone (not Chrome — Chrome's "Add to Home Screen" doesn't enable PWA mode).
2. Visit your GitHub Pages URL.
3. Tap the Share button → **Add to Home Screen** → name it "Protocol" → Add.
4. Open from the home screen icon. It runs fullscreen, no Safari chrome.
5. First open: tap **Enable Reminders** and allow notifications.

## 5. How data persists

- All state is stored in **localStorage** on the device under key `protocol-v1`.
- It persists across app launches, phone restarts, and Safari relaunches.
- It does **NOT** sync between devices. Open the app on iPhone only.
- iOS clears localStorage for PWAs after **7 days of no use**. Open it daily or weekly to keep data alive.
- The **Export Backup** button downloads a JSON snapshot. Do this weekly as insurance.

## 6. Offline behaviour

- After first load, the service worker caches the app shell.
- Open it on a plane, in the gym, anywhere — it works without internet.
- Updates: if you change the code and push to GitHub, force-quit the app and reopen to get the new version.

## 7. Notifications — important caveat

iOS Safari PWA notifications only fire when the app is **open or recently backgrounded**. If you force-quit, scheduled reminders won't trigger.

**Recommended pattern:**
- Open the app first thing on waking (it'll schedule the day's reminders).
- Leave it backgrounded, not force-quit.
- The 8 AM reminder will fire on its own.

For 100% reliable reminders, set native iOS alarms in the Clock app at each time block as a backup for the first week, then drop them once you trust the PWA reminders.

---

## Quick troubleshooting

| Problem | Fix |
|---|---|
| Blank page after upload | Wait 2 min — GitHub Pages takes time to deploy first time. Then hard-refresh (Cmd+Shift+R on desktop, or pull-to-refresh on mobile). |
| Reminders not firing | Check Settings → Notifications → Protocol → Allow. Re-tap "Enable Reminders" in the app. |
| Data disappeared | iOS cleared localStorage after 7+ days of disuse. This is why you Export Backup weekly. |
| Service worker stale | In iPhone Settings → Safari → Advanced → Website Data → search "github" → Remove. Then reopen. |

---

## Day counter logic

- Protocol start: **23 May 2026** (hardcoded).
- Day number auto-calculates from today's date.
- "Mark Day Complete" requires 100% items checked. It increments the counter and resets all checkboxes for the next day.
- Counter caps at Day 56.
