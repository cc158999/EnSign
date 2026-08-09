<div align="center">

<img src="assets/icons/yaolan.png" width="96">

# EnSIgn

**An all-in-one code-signing app for iOS — sign, inject, browse sources, manage files, all on device**<br>
**With one-tap auto update, so you never re-sign by hand again**

[简体中文](README.md) · English

![Platform](https://img.shields.io/badge/platform-iOS%2016.0%2B-black)
![Version](https://img.shields.io/badge/version-2.1.5-blue)
![Languages](https://img.shields.io/badge/UI-简体中文%20%7C%20繁體中文%20%7C%20English%20%7C%20Ti%E1%BA%BFng%20Vi%E1%BB%87t-green)

**📦 Unsigned IPA** → [pan.enqapp.com](https://pan.enqapp.com/)<br>
**🚀 Online signing & installation** → [www.enqapp.com/install](https://www.enqapp.com/install)

### Join the community

**[QQ Group](https://qun.qq.com/universal-share/share?ac=1&authKey=Pifbfqrq5x%2FgEZmQNlaU8DbgJyQeJga5hu9gaZTONxOIWgG7jaXr%2FMgydVWuzynv&busi_data=eyJncm91cENvZGUiOiI5NjIzMzc1MjMiLCJ0b2tlbiI6InlaUno4LzJvY2VBWXpEMVE1aXBlb082MTVlTFB1VUhlaitnQW5aYlhJaW1kM25VYUNBc0gvS0hhSFlIMGZPcEIiLCJ1aW4iOiIzMTIwNjE1NDM2In0%3D&data=0jUZUSdflWEWXWmg6l4vwK1HxgDpDhjqGumH32eKbuZ-X-ZyixgBbU4JELb-NdJwacP7EBYDP3KkQgY6Se6uhw&svctype=4&tempid=h5_group_info)** · **[QQ Channel](https://pd.qq.com/s/bceanzrgg?b=9)** · **[Telegram Channel](https://t.me/tbox_Sign)** · **[Telegram Group](https://t.me/tbox798)**

[Auto update](docs/auto-update.en.md) · [Features](docs/features.en.md) · [Getting started](docs/getting-started.en.md) · [FAQ](docs/faq.en.md) · [Changelog](docs/changelog.en.md)

</div>

---

## What is it

EnSIgn is a **local signing tool** that runs on your iPhone or iPad. It packs every step of "re-sign an IPA" into one native app: import a certificate, edit the app's metadata, inject tweaks, run the signing, install to the device, and manage files and updates afterwards.

No computer. No jailbreak. One phone takes you from a downloaded package to an icon on your home screen.

The design takes its cues from 轻松签 (QingSongQian): file import, certificate management, the signing flow and the install feedback all stay close to what long-time users already know — so switching over doesn't mean relearning everything.

<div align="center">
<img src="screenshots/signing.png" width="240"> <img src="screenshots/signing-tweaks.png" width="240"> <img src="screenshots/library-signed.png" width="240">
</div>

---

## ⭐ One-tap auto update

<img src="screenshots/auto-update.png" width="240" align="right">

A sideloaded app has no App Store update channel, so the usual routine is doing it all again by hand: find the new package → import → pick a certificate → re-sign → reinstall.

**EnSIgn collapses that into a single tap.**

```
Match the original certificate → submit re-signing job → server signs → ready to install
```

- **Completely free** — your own certificate or a developer certificate provided by EnSIgn, treated the same
- **Certificate matched automatically** — no remembering which one you used, no re-import
- **No duplicate queueing** — repeat submissions reuse the running job; a busy queue tells you to retry shortly
- **Independent from local signing** — everyday signing of third-party IPAs stays fully local and uploads nothing

Among comparable signing tools, auto update is one of the few real dividing lines. See **[Auto update →](docs/auto-update.en.md)**

<br clear="right">

---

## At a glance

| Module | What it does |
|---|---|
| **Auto update** | One tap for "match certificate → re-sign → install", free, no manual re-signing |
| **Signing** | Zsign-powered local re-signing with p12 + provisioning profile, ad-hoc, or modify-only; outputs IPA / TIPA |
| **App editing** | Change display name, bundle ID, version, icon; key-level Info.plist editing; app cloning |
| **Tweak injection** | Inject dylib / deb / framework; jailbreak dependencies rewritten to ElleKit; unresolved ones downgraded to weak links |
| **Certificates** | Import p12, provisioning profiles and encrypted bundles; revocation & expiry checks; cloud restore; URL-scheme import |
| **Library** | Signed / unsigned tabs, bulk import and delete, on-device HTTPS install server, QR-code install |
| **Sources** | AltStore / ESign repo formats, plus **unencrypted 全能签 (QuanNengQian) sources**; news, filtering, search, offline cache |
| **Files** | Full file browser, multi-format extraction, wireless transfer from a desktop browser, built-in downloader |
| **Editors** | Code/text editor with syntax highlighting, plist editor, hex editor, Assets.car viewer |
| **System integration** | Live Activity progress on Dynamic Island & Lock Screen, "Open in" import, URL scheme, Face ID lock |

Full breakdown: **[Feature list →](docs/features.en.md)**

---

## Screenshots

<div align="center">

| Signing | Tweak injection | Progress |
|:---:|:---:|:---:|
| <img src="screenshots/signing.png" width="220"> | <img src="screenshots/signing-tweaks.png" width="220"> | <img src="screenshots/signing-progress.png" width="220"> |

| Library (signed) | Library (unsigned) | Sources |
|:---:|:---:|:---:|
| <img src="screenshots/library-signed.png" width="220"> | <img src="screenshots/library-unsigned.png" width="220"> | <img src="screenshots/sources.png" width="220"> |

| File manager | Plist editor | Assets viewer |
|:---:|:---:|:---:|
| <img src="screenshots/files.png" width="220"> | <img src="screenshots/plist-editor.png" width="220"> | <img src="screenshots/assets-viewer.png" width="220"> |

| Settings | Signing options | Appearance |
|:---:|:---:|:---:|
| <img src="screenshots/settings.png" width="220"> | <img src="screenshots/settings-signing.png" width="220"> | <img src="screenshots/settings-appearance.png" width="220"> |

</div>

> The screenshots show the Chinese UI; the app also ships with full English, Traditional Chinese and Vietnamese localisations.

More in [screenshots/](screenshots/)

---

## Getting started

1. **Install EnSIgn** — install directly from [online signing](https://www.enqapp.com/install), or grab the [unsigned IPA](https://pan.enqapp.com/) and sign it yourself
2. **Import a certificate** — Settings → Certificates, pick your p12 + provisioning profile (or just open an `.ESignCert` bundle)
3. **Get a package** — download from a source, grab it with the built-in downloader, or "Copy to EnSIgn" from another app
4. **Sign** — pick the unsigned package in Library → Sign → adjust app info and tweaks → start
5. **Install** — installation is offered automatically when signing finishes, or install later from the signed list / via QR code on another device

> On iOS 16 and later you need to **enable Developer Mode** and trust the certificate under Settings → General → VPN & Device Management before a signed app will run.

Step-by-step: **[Getting started →](docs/getting-started.en.md)**

---

## Who it's for

- **Everyday iOS users** — manage your own IPA files and install test builds without depending on a computer
- **Developers and testers** — validate development builds, distribute internal betas, and inspect certificates, profiles and install status
- **Power users** — resource editing, tweak testing, file replacement, source management, and separate app copies for multiple accounts

> For long-term use, get a personal developer certificate or a dedicated P12. Shared certificates are fine for trying things out, but they're less stable — expect occasional expiry, revocation or device mismatch.

---

## Requirements

- iOS / iPadOS **16.0 or later**, adapted through the latest release (Live Activities need 16.2+)
- **iPhone 8 or newer**; iPad supported
- **No jailbreak required**
- A valid signing certificate (personal developer or enterprise)
- Interface languages: **简体中文 / 繁體中文 / English / Tiếng Việt**, four fully localised UIs, switchable at any time

### Four built-in icons

<div align="center">
<img src="assets/icons/yaolan.png" width="72"> <img src="assets/icons/zijing.png" width="72"> <img src="assets/icons/bili.png" width="72"> <img src="assets/icons/hupo.png" width="72">

Azure · Amethyst · Jade · Amber
</div>

---

## Documentation

- [Auto update](docs/auto-update.en.md) — the headline feature: how it works and where the privacy boundary sits
- [Feature list](docs/features.en.md) — every module, spelled out
- [Getting started](docs/getting-started.en.md) — from zero to your first installed app
- [FAQ](docs/faq.en.md) — signing failures, install errors, crashes
- [Release history](docs/changelog.en.md) — what changed from 1.x to 2.1.x

---

## Privacy

- **Everyday signing happens entirely on device** — certificates and packages never leave the phone
- **Auto update** is the one channel that involves the server (server-side re-signing); output is cleared on a timer and every request is ECDSA-verified. Skip it and stay fully local if you prefer
- Crash reporting and device statistics only start **after you accept the privacy policy**
- Certificate passwords are stored encrypted, never in plain text; logs never contain your UDID or passwords

---

## For developers and certificate vendors

EnSIgn exposes a one-tap **certificate import** interface (URL scheme `enq-app://`). Integration notes live in the app under **Settings → About → URL Scheme**, where you can also copy a complete AI integration spec and let an assistant write the integration for you.

---

## Terms of use

- EnSIgn is a **signing and file-management tool**. It does not provide or host any third-party application packages
- Use it in accordance with your local laws, and only to install apps you have the right to install (your own builds, internal betas, or resources you're authorised to use)
- Re-signing may breach the terms of service of some applications — assess that yourself before you do it
- Obtaining and using certificates, and complying with their terms, is the user's own responsibility

---

## Related projects

### [EnqAppstore](https://github.com/cc158999/EnqAppstore) — source hosting for EnSIgn

A **source (repo) server** by the same author, built for EnSIgn. It's a substantial fork of the BT Panel (宝塔) one-click deployment image `nsk_qnq_appstore`, aimed at giving EnSIgn users a more polished, more native source-hosting experience.

If you want to run your own source, distribute internal builds to a team, or turn a pile of IPAs into a subscribable repo, deploy it and add the resulting source straight into EnSIgn.

---

## Open-source notice

**EnSIgn is built on the following open-source projects:**

- **Ksign** — https://github.com/Nyasami/Ksign
- **Feather** — https://github.com/claration/Feather

Substantial further development and localisation work has been done on top of them. Thanks to the original authors.

## Credits

- [Zsign](https://github.com/zhlynn/zsign) — signing core
- [ElleKit](https://github.com/evelyneee/ellekit) — tweak injection runtime
- And everyone who uses and reports back ❤️

The complete open-source licence list lives in the app under Settings → About → Licences.
