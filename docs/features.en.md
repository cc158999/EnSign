# Feature list

[← Back to home](../README.en.md) · [简体中文](features.md)

---

## 0. One-tap auto update (headline feature)

A sideloaded app has no App Store update channel. EnSIgn collapses the whole "find package → import → pick certificate → re-sign → reinstall" routine into a single tap:

```
Match the original certificate → submit re-signing job → server signs → ready to install
```

- **Completely free**, whatever the certificate's origin — your own and EnSIgn-provided developer certificates are treated identically
- **Certificate matched automatically**, no need to recall which one you used
- A dedicated **staged progress screen**; failures land on their own error screen with a readable reason
- Once the install link is handed over, the app backgrounds itself so the system installer takes over
- Repeat submissions reuse the running job instead of starting a second server-side signing process; a busy queue says so explicitly
- Separate from local signing — everyday third-party IPA signing stays fully local

Details and privacy boundaries: **[Auto update](auto-update.en.md)**.

---

## 1. Signing

### Core

- Native re-signing powered by **Zsign**, running entirely on device — certificates and packages are never uploaded
- Three modes:
  - **Certificate signing** — normal re-sign with p12 + provisioning profile
  - **Ad-hoc signing** — temporary signing without a certificate
  - **Modify only, don't sign** — edit the package contents without touching the signature, handy for tweaking an already-signed build
- Output as **IPA** or **TIPA** (TrollStore package)

### Output control

- Custom output filename with a rule template; a timestamp is appended by default so nothing gets overwritten
- Selectable compression level (from "store, fastest" to "smallest"), trading packing speed against file size
- Packaging uses APFS cloning to avoid re-copying large payloads

### Batching and queue

- Multi-select in the Library to sign a batch in one go
- A processing-queue screen shows live status for every task
- Submitting the same task twice reuses the running job instead of spawning a second one

### App cloning

- Produce several copies in one pass, each with its own bundle identifier
- Clones live on the home screen alongside the original app

### Data security

- Optional Keychain Isolation gives signed apps separate keychain access groups, reducing data access between sideloaded apps

### Signing log

- Structured, stage-by-stage log (read package → edit config → inject → sign → package)
- Streams line by line, so you see progress while it runs
- Exportable and copyable in full — paste it straight into a bug report
- Precise timing down to milliseconds (e.g. `1.46s`)

---

## 2. Editing the app

- **Basics**: display name, bundle identifier, version and build number, all editable right on the signing screen
- **App icon**: replace from photo library or files
- **Info.plist key editing**: when the presets aren't enough, add / change / remove keys directly
- **Fix White Icon**: works around packages whose icon renders blank after install
- **Fix Dark Icon**: strips `UIPrerenderedIcon` for iOS 18 dark/tinted icons
- **Remove provisioning profile**: choose whether embedded.mobileprovision ends up in the output
- **Automatic cleanup**: removes stale `_CodeSignature`, drops invalid Watch placeholder apps, keeps extension bundle IDs in sync
- **Dependency inspection**: view the main binary's architectures and linked dylibs before signing

---

## 3. Tweak injection

- Injects **dylib / deb / framework** payloads
- **Tweak store**: a managed Tweaks directory — download tweaks from a source, pick them from the file manager, or share them in from another app
- **Tweak manager**: a real folder browser with search, file sizes, warnings, and true deletion of bare dylibs
- **Jailbreak dependency fixing**:
  - Dependencies are consolidated onto **ElleKit**, rewriting the tweak's `LC_LOAD_DYLIB` entries
  - Only tweaks that actually carry jailbreak dependencies get rewritten — nothing is injected unconditionally
  - Dependencies that can't be resolved are downgraded to **weak links** and logged, instead of crashing at launch
  - An existing runtime already inside the app is reused in place rather than duplicated
- **Removal**: list and remove already-injected dylibs, including ones sitting in the `.app` root (not just Frameworks)
- Every injection stage is written to the signing log so failures are easy to pinpoint

---

## 4. Certificate management

- **Import paths**:
  - p12 + mobileprovision pair
  - `.ESignCert` encrypted bundle (password included — just open it)
  - Bulk import from an archive, with a picker when it contains several certificates
  - **URL scheme import** (`enq-app://`) for certificate vendors to push a certificate from a web page. The app can also copy an integration spec for an AI assistant, so vendors can wire up the flow without reading docs
- **Deduplication**: only counts as a duplicate when both the certificate and the profile match; duplicates are rejected
- **Status checks**:
  - Revocation checking, refreshable on demand
  - Expiry date and days remaining shown as a pill
  - Common name, team and entitlements list all viewable
- **Cloud restore**: restore previously used certificates after a reinstall or a new device
- **Secure storage**: certificate passwords are encrypted at rest, never stored in plain text

---

## 5. Library

- Separate **Signed** and **Unsigned** tabs
- Each row shows icon, name, bundle ID, version, size and signing time
- **Bulk import** of multiple IPAs at once
- **Bulk delete** with a single confirmation step, so nothing goes by accident
- **Installing**:
  - Built-in **HTTPS install server** (self-signed TLS + `itms-services://`) — one tap installs
  - **QR-code install** — scan from another device to install there
  - Dedicated progress and error screens with readable failure reasons
- **Long-press menu**: open in file manager, share, view details
- Temporary IPA caches are cleaned up automatically after installation

---

## 6. Sources (App Store tab)

- Compatible with the **AltStore / ESign** repo formats, including encrypted and obfuscated sources
- Supports **全能签 (QuanNengQian) sources in their unencrypted format**, so an existing source list carries over as-is
- **Source management**: add a source in one step, land straight in the source list afterwards
- **News section** showing announcements published by the source
- **Filter and search**, plus a dedicated search tab
- **App detail pages**: iTunes screenshots and ratings, colour scheme derived from the app icon, skeleton loading
- **Downloads**:
  - Duplicate download prevention
  - Non-IPA files supported — dylib / deb / framework land in the Tweaks directory and are marked as downloaded
  - Download progress surfaces on the Dynamic Island
  - Web links inside a source open in the in-app browser instead of being force-downloaded as packages
- **Performance**: source data is cached on disk so cold starts don't refetch everything; the list is rendered through a UIKit bridge and stays smooth with tens of thousands of apps, with ProMotion refresh enabled
- **Host your own**: [EnqAppstore](https://github.com/cc158999/EnqAppstore), by the same author, is the matching source-hosting server — deploy it and add the resulting source straight into EnSIgn

---

## 7. File manager

- A full sandbox browser: create folders, rename, move, copy, delete, breadcrumb navigation, search, destination picker with conflict warnings
- **Extraction**:
  - `zip` / `tar` / `gz` / `xz` / `bz2` / `lzma` / `7z`, plus `ipa` / `tipa` / `deb`
  - Tapping a `.zip` extracts it directly instead of opening a preview
  - Progress overlay while extracting; never overwrites a same-named folder
- **File transfer**: turn it on and move files in and out from a desktop browser — no cable needed
- **Built-in downloader**:
  - Picks up links from the clipboard automatically
  - Sniffs IPA / TIPA / ZIP / dylib / deb and files them by date and type
  - Task list, with a jump to the result when a download finishes
- Image files preview as thumbnails; everything else goes through QuickLook

---

## 8. Editors and viewers

| Tool | Capabilities |
|---|---|
| **Code / text editor** | Runestone-based, line numbers and syntax highlighting (CSS / HTML / JSON / JavaScript / Markdown / Python / YAML); find, go-to-line, encoding switch, free font sizing, shortcut toolbar |
| **Plist editor** | Type-aware key/value editing, expand and collapse by tapping the row, flattened whole-tree search with match count, undo/redo |
| **Hex editor** | Paged loading for large files, undo/redo, read-only and edit modes |
| **Assets.car viewer** | Parses the asset catalog so you can inspect the images and their scales inside a package |

---

## 9. Settings

- **Appearance**: custom accent colour, light/dark following the system, four built-in app icons (Azure / Amethyst / Jade / Amber), Liquid Glass toggle on iOS 26
- **Signing options**: default options, compression level and filename rules in one place — and they survive app updates
- **Archive**: manage signed output archives
- **Storage**: per-category cache breakdown with individual cleanup (source cache, image cache, temporary files)
- **Language**: four fully localised interfaces — 简体中文 / 繁體中文 / English / Tiếng Việt — applied immediately on switch
- **Security**: Face ID / Touch ID app lock
- **Logs**: view and export runtime logs, following the system appearance
- **Support tickets**: submit anonymous tickets in-app with attachments and threaded replies, optionally attaching runtime logs
- **About**: version info, developer credit, website, and an auto-generated open-source licence list
- **Reset**: return to a clean state and rebuild the launch flow in place, without quitting the app

---

## 10. System integration

- **Live Activities**: signing and download progress on the Dynamic Island and Lock Screen, with quick install or sign actions when a task finishes
- **"Open in" import**: share IPA / TIPA / ZIP / DEB / dylib / framework / p12 / provisioning profile / `.ESignCert` into EnSIgn from any app's share sheet
- **URL scheme**: `enq-app://` supports adding sources, downloading resources and one-tap certificate import; see the [URL Scheme guide](url-scheme.en.md) for formats and examples
- **Background execution**: registered per task and released when a task stalls — it never squats in the background
- **OS compatibility**: iOS 16 through the latest release, with a fallback path for every newer API
- **Self-protection**: an integrity module that only warns on an abnormal environment — it never force-quits or crashes on purpose

---

[← Back to home](../README.en.md)
