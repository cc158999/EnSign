# URL Scheme guide

[← Back to home](../README.en.md) · [简体中文](url-scheme.md)

EnSIgn supports `enq-app://` links that start a specific action from a web page, Shortcut or another app. Its public actions can add a source, download a resource or import a signing certificate in one tap.

> Examples are also available in **Settings → About → URL Scheme** inside the app.

---

## Supported actions

| Action | Format | Typical use |
| --- | --- | --- |
| Add a source | `enq-app://source/[source URL]` | Subscribe directly from a source website |
| Download a resource | `enq-app://install/[resource URL]` | Hand an IPA / TIPA download to EnSIgn |
| Import a certificate | `enq-app://import-certificate?...` | One-tap import from a certificate service |

The link must be opened on an iPhone or iPad where EnSIgn is installed. On the web, launch it from an explicit user click; iOS or the browser may block automatic redirects during page load.

---

## Add a source

Place the complete HTTPS source URL after `/source/`:

```text
enq-app://source/https://example.com/appstore
```

This opens EnSIgn and starts the add-source flow. Keep the literal `https://` here; do not percent-encode it as `https%3A%2F%2F`.

---

## Download a resource

Place the resource URL after `/install/`:

```text
enq-app://install/https://example.com/MyApp.ipa
```

- `.ipa` and `.tipa` URLs create a download task
- Other web or resource URLs open in EnSIgn's in-app browser
- This is a compatibility entry point; downloading, signing and installation still follow the normal in-app flow

As with the source action, keep the complete literal `https://` URL.

---

## Import a certificate (recommended integration)

`enq-app://import-certificate` lets a certificate service hand a certificate to EnSIgn. After the user taps the link, EnSIgn shows the source and waits for confirmation before downloading or importing anything.

### Recommended: an `.ESignCert` bundle

```text
enq-app://import-certificate?url=https%3A%2F%2Fexample.com%2Fcertificate.ESignCert
```

`.ESignCert` is an encrypted certificate bundle supported by EnSIgn. It already carries the required information, so no certificate password needs to appear in the link. This is the preferred format for public web integrations.

### A ZIP bundle

The ZIP must contain both a `.p12` and a `.mobileprovision`; they may be inside subdirectories:

```text
enq-app://import-certificate?url=https%3A%2F%2Fexample.com%2Fcertificate.zip&password=YOUR_PASSWORD
```

### Separate certificate and profile URLs

```text
enq-app://import-certificate?p12=https%3A%2F%2Fexample.com%2Fcertificate.p12&mobileprovision=https%3A%2F%2Fexample.com%2Fprofile.mobileprovision&password=YOUR_PASSWORD
```

`provision` is accepted as an alias for `mobileprovision`.

### Inline Base64

```text
enq-app://import-certificate?p12=[P12_BASE64]&mobileprovision=[PROFILE_BASE64]&password=YOUR_PASSWORD
```

Use Base64 only as a compatibility fallback for small files. Safari, messaging apps and redirect pages often truncate long links, so remote file URLs should be the default.

### Parameters

| Parameter | Required | Description |
| --- | --- | --- |
| `url` | Either this or `p12` | HTTP(S) URL for an `.ESignCert` or ZIP bundle |
| `p12` | Either this or `url` | A `.p12` URL or Base64 content |
| `mobileprovision` | Required with `p12` | A provisioning-profile URL or Base64 content; alias: `provision` |
| `password` | Depends on the file | The p12 password, not Base64; unnecessary for `.ESignCert` |
| `name` | No | A display name for the imported certificate |
| `default` | No | Use `1`, `true` or `yes` to make it the default certificate |

If `url` and separate-file parameters are both present, EnSIgn uses `url`.

### Encode query values correctly

Certificate import uses query parameters, so **every parameter value must be percent-encoded**:

```javascript
const bundleURL = "https://example.com/certificate.ESignCert";
const schemeURL = `enq-app://import-certificate?url=${encodeURIComponent(bundleURL)}`;

document.querySelector("#import-certificate").addEventListener("click", () => {
  window.location.href = schemeURL;
});
```

Provide a button the user explicitly clicks:

```html
<button id="import-certificate" type="button">Import into EnSIgn</button>
```

Do not redirect during page load, and do not attempt to bypass EnSIgn's confirmation prompt.

### What the user sees

1. The website asks to open EnSIgn
2. EnSIgn identifies the source and asks for confirmation
3. Download and import start only after confirmation
4. A result message explains success or failure

Remote imports require the user to have accepted EnSIgn's privacy policy. If a password is missing or incorrect, the app prompts for another entry when the flow can recover.

### Server recommendations

- Prefer `.ESignCert` so a password never appears in the URL, browser history or access logs
- Use a one-time download token with a short expiry, invalidated after use
- Use HTTPS and return the correct filename and extension
- Keep remote files below 8 MB and decoded inline Base64 below 4 MB
- Never write certificates, passwords or complete import links to public logs

### Troubleshooting

| Symptom | Check |
| --- | --- |
| The link is incomplete | Required parameters and their spelling |
| Nothing happens | The action came from a user click and the messaging app did not truncate the link |
| Download fails | The HTTPS URL still works, returns 2xx and has not expired |
| Certificate files are missing | The ZIP contains both `.p12` and `.mobileprovision` |
| Password rejected | Use the original p12 password; do not Base64-encode it |

---

## Generate integration code with AI

In EnSIgn, open **Settings → About → URL Scheme → Import Certificate**, tap **Copy AI Integration Doc**, then paste it into ChatGPT or Claude and state the language or framework used by your website. The copied text includes the full parameter rules, security requirements and expected code output.

[← Back to home](../README.en.md)
