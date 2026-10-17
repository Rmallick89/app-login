# Shine Login — iPhone home-screen app

The logged-in Shine app (`App-login/`) as an iPhone home-screen web app — the iPhone version of the Android APK in
`../app-preview/`, built the same way as `App-logout/ios-app-git/` (the logged-out previews).

| Folder | App name | Opens |
|---|---|---|
| `login/` | Shine Login | splash → logged-in homepage (`App Dashboard/app-home-login.html`) · Jobs tab → `My Jobs/my-jobs.html` · Docket tab → `Docket/docket.html` |

The app opens on its splash (`splash.html`, dark midnight navy #0C1731, ≈ 2.4 s — two shooting stars meet at the logo,
then a sunrise glow rises from the bottom), which hands over to the logged-in homepage (`index.html`). The homepage
sends a fresh launch to the splash itself (once per session), so the installed icon always starts with the splash.
The Jobs tab opens My Jobs (`my-jobs.html`: Recommended · Saved · Applied, with the App-logout JSRP job cards) and the
Docket tab opens the Docket screen (`docket.html`); the tabs keep a native history (swipe back returns to the previous
tab). Menu, search, job/company/job-details pages, the check-in sheets and the Me tab aren't built yet — those taps stay
on their screen.
The bottom tab bar is the shared component `App-login/app-components/bottom-tab-bar` (v1.3.1): 104 pt tall on iPhones with a
home indicator — 12 top, 56 tab, 36 bottom (the 34 pt safe area + 2) — and 80 pt without one. Changing tab plays the
Stretch + Flip motion as the new screen opens (the grey wash stretches across, the icon turns over to filled; none with
Reduce Motion on).
Swiping back inside Docket › Expired or My Jobs › Saved › Expired returns to the list (shared sub-view helper
`App-login/app-components/sub-view`).
Every job on My Jobs and Docket is the shared job-card component (`App-login/app-components/job-card`, v1.1.0): active cards act, Applied / Action Taken cards only show what you did and when, expired cards are grey and open from the expired nudge.
The top bar is the shared top-app-bar component `App-login/app-components/top-app-bar` (v1.0.3): Home's floating brand
bar and the title bar on My Jobs / Docket share one frame, so the menu (menu-button v1.1.0, always first), the logo /
title and the right edge sit on the same spot on every tab. Titles are 20 / 24 bold. On iPhone the title bar is the standard 44 pt bar right under the status bar, nothing in the header moves
while a list scrolls, and the bar keeps the page colour so it always matches the status bar; a hairline marks the scroll edge (the menu itself isn't built yet).
The Searches / Actions switcher on Home and the Docket / My Jobs tabs are the shared segment-control component
`App-login/app-components/segment-control` (one 40 px control everywhere; v1.1 stretch switch motion —
the thumb stretches to the tapped option, none with Reduce Motion on).
Docket's and My Jobs' "Remove this saved job?" confirmation is the shared bottom-sheet component `App-login/app-components/bottom-sheet`
(inline buttons; on iPhones with a home indicator they sit 16 pt above the 34 pt safe area).

**Launch screen:** iPhone shows a launch screen before a home-screen app has loaded. The app ships plain #0C1731 launch
images (`icons/launch-*.png`, one exact size per iPhone screen, linked as `apple-touch-startup-image`), the same colour
as the splash's first frame, so tapping the icon shows only the Shine splash.

`login/` is a self-contained app: pages, fonts, a web-app manifest, home-screen icons (midnight navy, white Shine logo)
and an offline cache (`sw.js`). `index.html` at the root is the install page.

## Publish (GitHub Pages)
This folder is its own git repo (branch `main`, no commits yet).
1. On GitHub create an empty repo named `ios-preview`.
2. From this folder: `git add -A`, `git commit -m "Shine Login iPhone preview"`,
   `git remote add origin https://github.com/Rmallick89/ios-preview.git`, `git push -u origin main`
   (the password is a personal access token).
3. On GitHub: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / root → Save**.
4. After a minute the install page is at `https://rmallick89.github.io/ios-preview/`.

## Install on an iPhone
Open the install page in **Safari** → tap **Shine Login** → **Share → Add to Home Screen → Add**. Open it once while
online; after that it also works offline. It sits next to the logged-out previews (Shine Preview / Folder / Cards).

## Updating
The pages are copied from the Android APK so both platforms show the same build. After App-login changes and
`../app-preview/app-login.apk` is rebuilt, run `python3 build_ios.py ../app-preview .` here (it refreshes `login/`
and the offline cache version, keeping the icons), then commit and push. The installed app picks up the new version the
next time it's opened online. If the launch colour changes, delete `login/icons/launch-*.png` first (the script only
creates missing ones), then delete the home-screen icon and add it again (iOS saves the launch screen when the app is added).

Limits on iPhone: no tap vibration (iOS doesn't allow it for web apps); the status bar is the standard light bar.
