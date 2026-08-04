# FAQ

[← Back to home](../README.en.md) · [简体中文](faq.md)

---

## Auto update

**What is auto update, and does it cost anything?**
Once installed, you never re-sign by hand. Tap update and EnSIgn matches the certificate you installed with → submits a re-signing job → the server signs it → it's ready to install. **Completely free**, whether the certificate is your own or one provided by EnSIgn. See [Auto update](auto-update.en.md).

**Is auto update the same thing as local signing?**
No — they're separate paths. Auto update goes through the server-side re-signing channel; everyday signing of third-party IPAs stays fully local and uploads nothing. You can use local signing exclusively and never touch auto update.

**Do I have to pick a certificate again when updating?**
No. The certificate you originally installed with is reused automatically.

**I tapped update and nothing happens / it says the queue is busy.**
Tapping again doesn't queue a second job — the running one is reused. When the server queue is busy you get an explicit "try again shortly"; wait a moment and tap again.

---

## Migrating from 轻松签

**How is EnSIgn related to 轻松签?**
There's no formal relationship. EnSIgn's design takes its cues from it: file import, certificate management, the signing flow and the install feedback all stay close to what long-time users know, so switching over doesn't mean relearning a whole new path.

**Can I keep using my existing sources?**
Yes. Alongside the AltStore / ESign formats, EnSIgn supports **全能签 (QuanNengQian) sources in their unencrypted format**, so your source list carries over as-is.

**What should I prepare before migrating?**
Three things, ideally ahead of time: your **certificates** (p12 + profile, or an `.ESignCert` bundle), your **frequently used IPA files**, and your **source URLs**. Waiting until the old tool expires makes for a rushed switch.

---

## Setup and environment

**Do I need a jailbreak?**
No. EnSIgn itself and everything it signs run on a stock, non-jailbroken system. Tweak injection uses the ElleKit runtime bundled into the package, so it doesn't depend on a jailbreak either.

**Which OS versions and devices are supported?**
iOS / iPadOS 16.0 and later, adapted through the latest release; iPhone 8 or newer, and iPad. Dynamic Island and Lock Screen Live Activities need 16.2+.

**Where do I download EnSIgn itself?**
Install directly from [www.enqapp.com/install](https://www.enqapp.com/install), or grab the unsigned IPA at [pan.enqapp.com](https://pan.enqapp.com/) and sign it yourself.

**Which languages?**
简体中文, 繁體中文, English and Tiếng Việt (Vietnamese) — four fully localised interfaces. Switch under Settings → Language; it applies immediately.

---

## Certificates

**My certificate won't import, or it says duplicate.**
- It only counts as a duplicate when **both** the certificate and the profile match. If you genuinely replaced one half, re-select both files
- A p12 prompts for its password; an `.ESignCert` bundle carries its own — don't type one
- The import path rejects IPAs and oversized archives, so don't pick a package by mistake

**How do I know a certificate still works?**
The certificate list shows expiry date, days remaining and revocation status, and revocation can be re-checked on demand. **A package signed with a revoked certificate installs fine but crashes on launch.**

**I lost my certificates after reinstalling.**
Use cloud restore to recover previously used certificates, or simply re-open your `.ESignCert` bundle.

**Are shared certificates good enough?**
Fine for trying things out, not for the long run. Shared certificates tend to be less stable — expiry, revocation and device mismatches happen. For sustained use, get a personal developer certificate or a dedicated P12.

---

## Signing

**How do I debug a signing failure?**
The signing log is staged (read package → edit config → inject → sign → package). Look at where it stopped:

- **Edit config** — usually a malformed package, or a broken Info.plist edit
- **Inject** — architecture mismatch between the tweak and the target app, or unresolved dependencies (the log names anything downgraded to a weak link)
- **Sign** — a certificate or profile problem; check the certificate screen first
- **Package** — most often the device is out of storage

The full log can be exported; attaching it makes support much faster.

**It signed, but it won't install.**
- On iOS 16 and later, Developer Mode must be enabled first (Settings → Privacy & Security → Developer Mode)
- A first install needs the certificate trusted under Settings → General → VPN & Device Management
- The device's UDID must be in the provisioning profile
- The install screen states a readable failure reason — follow it

**It installed but crashes immediately.**
Three usual causes: a revoked certificate, a profile that doesn't include this device's UDID, or an injected tweak that isn't compatible with the app. Re-sign once with injection turned off to tell a signing problem from a tweak problem.

**What is "modify only, don't sign" for?**
It edits the package contents (name, bundle ID, Info.plist and so on) without re-signing — useful for tweaking an already-signed build. Note that the output does **not** gain a fresh valid signature just because you changed something.

**How does app cloning work?**
One signing run produces several copies, each with a different bundle identifier, so the system treats them as separate apps that coexist with the original.

---

## Tweak injection

**The app won't launch after injecting a tweak.**
First check that the tweak targets that app and that architecture. The log names any dependency that couldn't be resolved and was downgraded to a weak link — a weak link only keeps it from crashing at launch, it doesn't guarantee the tweak works.

**Why is everything rewritten to ElleKit?**
Jailbreak tweaks usually link against a Substrate-style runtime that doesn't exist outside a jailbreak. EnSIgn rewrites those dependencies to point at the ElleKit runtime bundled into the package — currently the most reliable path on a stock system. Only tweaks that actually carry such dependencies get rewritten; nothing is injected unconditionally.

**Can I host my own source?**
Yes. [EnqAppstore](https://github.com/cc158999/EnqAppstore), maintained by the same author, is the matching source-hosting server — a substantial fork of the BT Panel one-click image `nsk_qnq_appstore`, built specifically for EnSIgn. Deploy it, then add the source URL in the app.

---

## Tweak injection

**Where do tweaks live?**
In the Tweaks directory. Download them from a source, pick them from the file manager, or share them in from another app. The tweak manager supports search, file sizes and full deletion.

---

## Data and privacy

**Is my certificate uploaded anywhere?**
Not for everyday signing — that happens entirely on device, and certificates and packages never leave the phone.

The one exception is **one-tap auto update**: it needs the server to perform the signing, so that channel does involve your certificate. Server-side output is cleared automatically on a timer, and every request and response is ECDSA-verified. Skip that channel and use local signing if you'd rather your certificate never left the device.

**What data is collected?**
Crash reports and basic device statistics — and reporting only starts **after you accept the privacy policy**. Logs never contain your UDID or certificate passwords, and passwords are stored encrypted rather than in plain text.

**Storage keeps growing.**
Settings → Storage breaks usage down by category — source cache, image cache, temporary files, signed output archives — and each can be cleared individually.

---

## Other

**My settings were reset after an app update.**
Signing options are persisted across updates. If you still hit this, please report it with your version number.

**Where do I report problems?**
Settings → Feedback lets you file an anonymous ticket with attachments and threaded replies, and optionally attach your runtime log.

[← Back to home](../README.en.md)
