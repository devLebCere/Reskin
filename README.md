<p align="center">
  <img src="https://raw.githubusercontent.com/mrYouki/YoukiShell-Android-Desktop/main/app/src/main/ic_launcher-playstore.png" width="220" alt="YoukiShell Logo"/>
</p>

<h1 align="center">YoukiShell</h1>

<p align="center">
  Build your dream in plugin
</p>

<p align="center">
  <a href="https://github.com/mrYouki/YoukiShell-Android-Desktop/releases">
    <img src="https://img.shields.io/github/v/release/mrYouki/YoukiShell-Android-Desktop?style=for-the-badge" alt="Latest release"/>
  </a>
  <a href="https://github.com/mrYouki/YoukiShell-Android-Desktop/blob/main/LICENSE">
    <img src="https://img.shields.io/github/license/mrYouki/YoukiShell-Android-Desktop?style=for-the-badge" alt="License"/>
  </a>
  <a href="https://github.com/mrYouki/YoukiShell-Android-Desktop/releases/latest">
    <img src="https://img.shields.io/badge/Download-APK-brightgreen?style=for-the-badge&logo=android" alt="Download APK"/>
  </a>
  <img src="https://img.shields.io/badge/Android-8.0%2B-green?style=for-the-badge&logo=android" alt="Android"/>
</p>

---

## What is this?

YoukiShell is an Android app you can shape into whatever you want it to be, through plugins.

Out of the box, it already turns your phone into something closer to a desktop — a taskbar, an app dock, floating windows you can resize and move around, hot corners, that whole experience. But the desktop mode is really just the first thing built on top of it, not the whole point of the app. Underneath, YoukiShell is a small engine that can load real, independently-built plugins at runtime — plugins get their own compiled code, their own native (C++) libraries if they need them, and their own compiled resources, the same building blocks a normal standalone app would have. A plugin isn't limited to adding a menu item somewhere; if a plugin author wants to replace an entire part of the app with something completely different, the engine lets them.

There's a built-in store (the **Workshop**) right inside the app, so finding and installing plugins — along with wallpapers and fonts — doesn't mean leaving YoukiShell or hunting around the web first.

A few features — custom display size, freezing background apps, that kind of thing — need Root or Shizuku. Everything else works out of the box.

---

## How it actually works

**1. The shell** — YoukiShell itself runs as an Accessibility Service overlay. That's what lets it draw a dock, floating windows, and the rest of the desktop experience on top of whatever app is open, without needing to be your default launcher (though most people set it as one anyway, just so it loads automatically).

**2. The plugin engine** — Underneath the shell sits a small runtime that can load a plugin's own compiled code (`.dex`), and connect it to the plugin's own native libraries and compiled resources if it ships any. A plugin runs as trusted, in-process code once loaded — it isn't sandboxed away in its own little box with a fixed set of things it's allowed to touch. That's the deliberate trade-off: less isolation, a lot more freedom for whoever's building the plugin.

**3. The Workshop (built-in store)** — A screen inside the app where plugins, wallpapers, and fonts show up as browsable, installable entries. Install a plugin from there and the engine picks it up immediately — no separate installer, no leaving the app.

**4. You decide what a plugin becomes** — a small settings screen, a full alternate home screen, a game, a utility that replaces one of the shell's own built-in parts — the engine doesn't limit that shape ahead of time. If a plugin author can build it and compile it, the engine can load it.

---

## About the project

Just one person building this in their free time — no team, no company. Everything gets written and built directly on a phone (a mid-range one — Unisoc T606, 8GB RAM) using Android Code Studio (ACS). No laptop involved at any point. It also gets tested on an itel S23, mostly to make sure nothing that only shows up on a budget-tier phone slips through.

That setup naturally shapes how things move: updates go out when something's actually ready, not on a schedule, and how fast bugs get fixed depends on how much free time there is that week.

---

<p align="center">
  <a href="https://discord.gg/mKkaMxd5M2">
    <img src="https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Join our Discord"/>
  </a>
</p>

<p align="center">
  <i>btw. Beta updates, beta source code, drama, and 3 AM debates about app icons all move faster on the Discord server — GitHub Issues is where bugs go to nap.</i>
</p>

---

For release notes and version history, check the [GitHub Releases page](https://github.com/mrYouki/YoukiShell-Android-Desktop/releases) — every release has its own detailed changelog.

---

## Features

**Workshop — the built-in store**
- Browse and install plugins directly inside the app — no separate download-then-install step
- A built-in browser for finding wallpapers and fonts too, with its own ad blocker (strips ads/popups live as the page loads) and automatic font capture — browse Google Fonts and it grabs and installs whatever font file the page loads
- Comes with a few sources out of the box (MotionBGs, DesktopHut, MoeWalls for wallpapers; Google Fonts, DaFont, Fontesk for fonts) — add your own by URL

**Plugin engine**
- Loads a plugin's own compiled code at runtime, with support for the plugin shipping its own native (C++) libraries and its own compiled resources (real XML layouts, not just hand-built views)
- Install, uninstall, and toggle each plugin individually
- A plugin can add a screen, replace an existing part of the app, or be something else entirely — the engine doesn't fix that shape in advance

**With Root or Shizuku**
- Custom display size/resolution, disabling heads-up notifications, freezing background apps, uninstalling system apps, soft reboot
- **Auto-grant** — the moment either is detected, the app grants itself everything it needs in one shot (over 20 permissions and system settings) instead of asking permission by permission. Accessibility stays a manual step on purpose, so it's always something you turn on yourself
- A built-in shell terminal

**Self-test tool**
- A QA feature (mostly for me) that opens every screen in the app on an isolated virtual display to catch crashes before a release goes out

---

## Under the hood

Kotlin, all the way through — the services, the UI, the database, the live wallpaper rendering, and the plugin-loading engine.

Runs on Android 8.0 and up, built with Gradle.

---

## Quick Start

1. Install the APK and open YoukiShell
2. Grant the permissions it asks for — Accessibility is the one that actually matters, nothing draws without it
3. Then either add the Quick Settings tile and tap it, set YoukiShell as your launcher, or use the home-screen shortcut it offers on first launch
4. Open the Workshop from Settings to browse and install plugins, wallpapers, or fonts whenever you want to add something new

---

## Optional: Root or Shizuku

A few features need `WRITE_SECURE_SETTINGS`-level access. Three ways to get there:

- **Shizuku** (easiest long-term) — install it, start it once, then grant YoukiShell permission when it asks.
- **Root** — Magisk or KernelSU, detected automatically, nothing to set up.
- **ADB**, if you'd rather do it manually and don't have either:
  ```bash
  adb shell pm grant com.youki.shell android.permission.WRITE_SECURE_SETTINGS
  adb shell appops set com.youki.shell GET_USAGE_STATS allow
  ```

Without any of these, the app still works — you just won't have the handful of features that need system-level access.

---

## Permissions

| Permission | What it's for |
|---|---|
| `SYSTEM_ALERT_WINDOW` | Draw the dock and floating windows over other apps |
| Accessibility Service | The core of how the overlay works |
| `PACKAGE_USAGE_STATS` | Show recent and running apps |
| `WRITE_SECURE_SETTINGS` / `WRITE_SETTINGS` | System-level settings (Root/Shizuku/ADB only) |
| `QUERY_ALL_PACKAGES` | List installed apps |
| Notification Listener | Show and manage notifications from the dock |
| `MANAGE_USERS` | Multi-user switching |
| `MANAGE_EXTERNAL_STORAGE` / media permissions | Wallpapers, Workshop downloads, backup/restore |
| `BLUETOOTH*` | Bluetooth indicator in the tray |
| `CAPTURE_VIDEO_OUTPUT` | Self-test tool's virtual display (Root/Shizuku only) |

---

## Known issues

**RedMagic & ZTE phones don't do floating windows out of the box.** These brands turn Freeform Windows off by default. Fix:
1. Enable Developer Options (tap Build Number 7 times)
2. Turn on **Force activities to be resizable** and **Enable freeform windows**
3. Go to **Settings → Display → Desktop Mode** and enable it
4. Reboot

Not a YoukiShell bug — that's just how those phones ship.

---

## FAQ

**Do I have to set it as my default launcher?**
No, it works either way — but if you don't, you'll need to open it by hand each time instead of it loading on boot. Most people just set it as default.

**The dock disappeared after a reboot.**
Check that Accessibility is still turned on, and that your phone's battery settings aren't killing it in the background — common on MIUI, ColorOS, and similar.

**What's a plugin, actually?**
A separately-built piece of compiled code (with its own resources and native libraries if it needs them) that YoukiShell loads at runtime, after you install it from the Workshop. It runs as trusted code inside the app rather than in a locked-down sandbox, which is what lets a plugin do more than just add a settings row — it can add a whole new screen, or take over part of the app entirely.

**Is this fully open source?**
Yes. Everything is built and pushed straight from a phone using Android Code Studio (ACS) — no computer involved anywhere in the process. Contributions and bug reports are always welcome.

**Accessibility won't turn on for me on Android 13+.**
That's Android blocking it because the app wasn't installed from the Play Store. One-time fix:
```bash
adb shell appops set com.youki.shell REQUEST_INSTALL_PACKAGES allow
```
Then go back to Settings → Accessibility and it should work. Installing via `adb install` instead of a regular APK also skips this entirely.

---

## Disclaimer

Independent, one-person project. Not affiliated with or endorsed by anyone.

---

## Credits

YoukiShell is built on top of **YoukiDex**, rebuilt from there into its current form — new package, new plugin engine, a lot of features added since. Started even further back from **Smart Dock**, though not much of that original codebase is left besides the initial idea at this point.

Thanks to the open-source Android community for the tools and libraries this leans on.

---

<p align="center">Made by <a href="https://github.com/mrYouki">mrYouki</a></p>
