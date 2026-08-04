# Getting started

[← Back to home](../README.en.md) · [简体中文](getting-started.md)

From nothing to your first signed app on the home screen.

---

## Step 0: What you need

- An iOS / iPadOS device on **16.0 or later**, **iPhone 8 or newer**, or an iPad (no jailbreak)
- A valid signing certificate — a p12 file plus a mobileprovision profile, or an `.ESignCert` bundle from a certificate vendor
- The package you want to install (IPA / TIPA), or a source you can download it from

Two ways to install EnSIgn itself:

| Entry point | What it is |
|---|---|
| [www.enqapp.com/install](https://www.enqapp.com/install) | Online signing and installation — straight from the web page, the easiest route |
| [pan.enqapp.com](https://pan.enqapp.com/) | The unsigned IPA, to sign with your own certificate |

> On iOS 16 and later you must **enable Developer Mode** (Settings → Privacy & Security → Developer Mode) and trust the certificate under Settings → General → VPN & Device Management before a signed app will launch.

---

## Step 1: Import a certificate

<img src="../screenshots/settings.png" width="240" align="right">

Go to **Settings → Certificates** and tap `+`. Several ways in:

- **Pick files** — opens the system file picker; select the p12 and the profile, and enter the password when prompted
- **Open an `.ESignCert`** — these bundles carry their own password; open one from the Files app and choose EnSIgn
- **Archive** — if it contains several certificates you get to pick one
- **One-tap from the web** — vendors supporting `enq-app://` can push a certificate straight in
- **Cloud restore** — recover a previously used certificate after a reinstall or new device

The list then shows the certificate name, expiry date, days remaining and revocation status. **Check the status before signing** — a package signed with a revoked certificate will just crash on launch.

<br clear="right">

---

## Step 2: Get a package

Three routes, pick whichever suits:

| Route | How |
|---|---|
| **Sources** | App Store tab → add a source → find the app → Get (AltStore / ESign formats, plus unencrypted 全能签 sources) |
| **Built-in downloader** | Download entry at the top of the Files screen; paste a direct link (the clipboard is read automatically) and it files the result for you |
| **Share it in** | Choose EnSIgn from any app's share sheet, or long-press in the Files app → Copy to EnSIgn |

Once imported, the package shows up under **Library → Unsigned**.

---

## Step 3: Sign

<img src="../screenshots/signing.png" width="240" align="right">

Select the package in the unsigned list to open the signing screen. What you can adjust:

- **App info** — name, bundle ID, version, icon; tap a row to edit
- **Certificate** — choose which one to use, or switch to "modify only, don't sign" here
- **Quick options** — remove provisioning profile, fix white / dark icon, fix jailbreak dependencies, app cloning, and more
- **Tweaks** — select the dylib / deb / framework payloads to inject
- **Output** — output filename, and whether to produce an IPA or a TIPA

When in doubt, leave the defaults — they work for the vast majority of packages.

Tap start. While it runs:

- A progress bar sits at the top, with the streaming signing log below it
- The Dynamic Island and Lock Screen show live progress, so you can switch away
- Total elapsed time is shown when it finishes

<br clear="right">

---

## Step 4: Install

The result lands in **Library → Signed**. Tap install:

- **On this device** — the app starts a local HTTPS server and hands off to the system installer
- **QR code** — scan from another device to install it there

On a first install you may need to trust the certificate under **Settings → General → VPN & Device Management**.

---

## Step 5: Updates are one tap from here on

Once installed, you never re-sign by hand again. When a new version appears, tap update — EnSIgn matches the certificate you installed with, submits the re-signing job, completes the signing and prepares the install, with a staged progress screen throughout.

This is **completely free**, whether you use your own certificate or one provided by EnSIgn. See [Auto update](auto-update.en.md).

---

## Handy things

- **Batch signing**: multi-select in the Library to queue several at once; the queue screen shows per-task status
- **App cloning**: enable it in quick options to sign several independent copies that coexist with the original
- **Filename rules**: Settings → Signing options lets you customise the output filename template (timestamped by default)
- **Wireless transfer**: turn on file transfer in the Files screen and open the shown address in a desktop browser
- **When something breaks, read the log**: both the signing log and the runtime log can be exported — attaching them makes support far faster

---

## Next

- [Auto update](auto-update.en.md) — the first thing to know once you're set up
- [Feature list](features.en.md) — everything else the app can do
- [FAQ](faq.en.md) — signing failures, install errors, crashes

[← Back to home](../README.en.md)
