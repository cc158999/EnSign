# Release history

[← Back to home](../README.en.md) · [简体中文](changelog.md)

Milestones by development phase rather than a commit-by-commit log. Current version: **2.1.3**.

---

## Origins: Feather and Ksign

EnSIgn's codebase started from the open-source signing tool [Feather](https://github.com/claration/Feather), with [Ksign](https://github.com/Nyasami/Ksign) as its direct upstream. Inherited from upstream: the Zsign signing core, the AltStore-format sources tab, the file browser, bulk import and delete in the library, certificate revocation checks, the "modify only, don't sign" option, accent-colour theming, and alternate app icons.

---

## Early 2.0.x (first half of 2026)

This phase turned a general-purpose upstream tool into a complete signing app for Chinese-speaking users.

**Signing and injection**
- The tweak injection system took shape: dylib / deb / framework payloads with grouped management
- Jailbreak dependency handling moved from unconditional injection to dependency-driven rewriting, and was then consolidated onto **ElleKit** — the Substrate path was retired entirely
- Added "Fix White Icon" and "Fix Dark Icon" (iOS 18) options
- The signing log was rebuilt into a clean structured format, isolated from raw zsign output and rendered line by line as it streams

**Editing and viewing**
- The text editor adopted Runestone with line numbers and multi-language syntax highlighting, plus find, go-to-line, encoding switching and free font sizing
- The plist and hex editors were rebuilt with paging, undo/redo and type awareness
- Added an Asset Catalog (`.car`) viewer

**Files and downloads**
- The file browser was reworked with a destination picker, folder creation, breadcrumbs and conflict warnings
- The downloader moved out of its own tab into the Files screen, became a task list, picks up links from the clipboard and files results by date and type
- The wireless file-transfer screen was redesigned

**Certificates**
- The certificate screen was rebuilt: list layout, automatic status detection, expiry and entitlement display
- Added URL-scheme certificate import (`enq-app://`) and a copyable AI integration spec
- Added cloud certificate restore; import now rejects IPAs and oversized archives and lets you pick from multi-certificate bundles

**System integration and localisation**
- Share-sheet import moved from a Share Extension to **Document Types / "Open in"**
- Live Activities on the Dynamic Island and Lock Screen show signing progress, with throttled updates
- The language set was organised into 简体中文 / 繁體中文 / English / Tiếng Việt (Vietnamese), applied instantly on switch
- Biometric lock, a privacy-policy gate, and crash reporting that only initialises after consent

---

## 2.0.7 – 2.0.9 (July 2026)

- **One-tap auto update shipped** — matches the installed certificate, submits a re-signing job, completes signing server-side and prepares the install, with a staged progress screen and a dedicated error screen; free for every user
- Source import gained progress and result reporting; the unlock flow was rewritten, with better refresh indication and request-queue management
- Major sources overhaul: on-disk caching removes the full refetch on cold start, first paint decodes concurrently and streams rows in, and web links open in the in-app browser
- The storage screen can inspect and clear the source cache
- Appearance settings can switch the app icon (Azure / Amethyst / Jade / Amber)
- The signing screen gained an Output section: custom output filename and IPA / TIPA choice
- Filename rules now include a timestamp by default
- Fixed signing options being reset after an app update
- Backend services came online: an online re-signing channel, source and version management, device statistics, and a UDID retrieval page

---

## 2.1.x (late July 2026 onwards)

**Productisation**
- The display name became **EnSIgn**, with InfoPlist localisation completed
- The About screen was rebuilt: credits, website link, and an auto-generated open-source licence list via LicensePlist
- Settings, Files and Sources were unified on one type scale and grouping style
- Added an in-app **anonymous ticket** system with attachments and threaded replies

**Features**
- **App cloning**: produce several independent copies in one signing run, with batch output
- QR-code install for signed apps
- The file manager extracts `tar` / `gz` / `xz` / `bz2` / `lzma` / `7z`, and opens IPA / TIPA directly
- Sources can download non-IPA files and mark them as downloaded; tweaks can be picked from the in-app file manager
- A "Fix jailbreak dependencies" toggle in quick settings, with copy that states plainly that it rewrites `LC_LOAD_DYLIB`
- A custom tab bar on iOS 16 / 17, putting Search back beside Library on older systems

**Performance and stability**
- Packaging switched to APFS cloning and stopped flooding progress updates; three redundant costs removed from IPA import
- Background execution became per-task registration that releases itself when a task stalls
- Per-row overhead removed from the sources list, with ProMotion refresh enabled
- Adapted to the external-share file permission changes introduced in iOS 26
- Dynamic Island and Lock Screen cards restyled, with brand badge and expanded-state fixes

---

[← Back to home](../README.en.md)
