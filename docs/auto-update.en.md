# One-tap auto update

[← Back to home](../README.en.md) · [简体中文](auto-update.md)

> The clearest thing that sets EnSIgn apart from other signing tools — and it's **completely free**.

---

## The problem it solves

An app installed through a self-signing tool has no App Store update channel. The traditional route is: notice a new version → find the new package → import it → pick a certificate → sign it again → install it again. Every update means walking the whole flow by hand.

EnSIgn collapses that into a single tap.

---

## How it works

When a new version is available, tap update and the rest happens on its own:

```
Match the certificate you originally installed with
              ↓
        Submit a re-signing job
              ↓
     Server completes the signing
              ↓
   Ready to install → system installer
```

A dedicated progress screen shows which stage you're on. Failures land on their own error screen with a readable reason instead of a vague alert. Once the install link is handed over, the app backgrounds itself so the system installer can take over.

---

## Things worth knowing

**Free, regardless of where your certificate came from**
Whether you use your own certificate or a developer certificate provided by EnSIgn, auto update works the same way, with no strings attached.

**The certificate is matched automatically**
Updates reuse whichever certificate you originally installed with — no need to remember which one it was, and no re-import.

**Submitting twice doesn't queue twice**
Tapping the same update again reuses the running job rather than spawning a second signing process on the server. When the server queue is busy you get an explicit "try again shortly" instead of a silent hang.

**It's a separate path from local signing**
Auto update goes through the server-side re-signing channel. Everyday signing of third-party IPAs stays **fully local** and uploads nothing. The two are independent — you can use local signing exclusively and never touch auto update.

---

## Privacy

Auto update needs the server to perform the signing, so this channel does involve your certificate. The relevant design:

- Server-side re-signing output is **cleaned up automatically on a timer**, not retained long-term
- Requests and responses are verified with **ECDSA signatures**, so the channel can't be tampered with
- If you'd rather not use it, stick to local signing and your certificate never leaves the device

---

## For developers and certificate vendors

EnSIgn exposes an interface for **importing a certificate into the app in one tap** (URL scheme `enq-app://`) — one click on a web page pushes the certificate in, with no file hunting or password typing for the user.

Integration notes live in the app under **Settings → About → URL Scheme**. That screen also offers "copy AI integration spec", which puts the full specification on your clipboard so an AI assistant can write the integration for you.

---

[← Back to home](../README.en.md) · [Feature list](features.en.md)
