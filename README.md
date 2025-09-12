# Happy Hour Housesitter — Signup Page

A simple, single-page site for friends & family to opt into housesitting on specific **2026** date windows. Visitors pick a date card, check the perks, and submit an interest form (via Formspree).

---

## What’s in this repo

- `index.html` — the entire site (HTML, CSS, and a tiny bit of JS)
- `HH-pano.jpg` — hero image displayed at the top (replace with your own if desired)
- `README.md` — this file

---

## Quick start (local preview)

1. Download this repo (or clone it).
2. Open `index.html` in any modern browser — you’ll see the live page.
   - Optional: if you use VS Code, the “Live Server” extension auto-reloads on save.

---

## Deploying to GitHub Pages

1. Create a new GitHub repo and add these files (`index.html`, `HH-pano.jpg`, `README.md`).
2. Commit & push.
3. In your repo, go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
5. Choose **Branch: `main`** and **Folder: `/ (root)`**, then **Save**.
6. Wait a minute and refresh; your Pages URL will appear at the top of the **Pages** section.

> Tip: Any time you push a change to `main`, GitHub Pages redeploys automatically.

---

## Editing the date windows & perks

Each date window is a “date card” block in `index.html`:

```html
<div class="date-card">
  <h3>January 19 – February 27, 2026</h3>
  <div class="perks">Cats AND avocados and citrus fruit...</div>
  <button type="button" data-date="January 19 – February 27, 2026">I'm Interested</button>
</div>
```

- **Keep the `<h3>` text and the `data-date` value in sync.**  
  The button writes the `data-date` value into the form’s read-only “Selected Date Window” field.
- To add another card, copy/paste one block and adjust dates/perks.
- To remove a card, delete the whole `<div class="date-card">…</div>` block.

**Current windows in this version**  
- January 19 – February 27, 2026  
- April 10 – April 26, 2026  
- **June 6 – July 8, 2026** (fixed to be consistent across heading and button)

---

## Form handling (Formspree)

The interest form submits to Formspree using `fetch` (AJAX), with an inline success/error message bar (no alerts).  
- Current endpoint: `https://formspree.io/f/xldlvegl`
- Fields submitted: **Date Window**, **Name**, **Email**, **Phone (optional)**, **Message**, plus anti-spam fields (see below).

### Update your Formspree endpoint
If you create a new Formspree form, replace the `action` on the `<form>` tag with your new endpoint URL.

### Success / error UX
A small status bar appears above the form (with `aria-live="polite"`) to announce success or errors. You can tweak the messages/styles in the `<style>` block and `showStatus()` function.

---

## Reducing spam

This page uses **two layers** out of the box:

### 1) Honeypot (always on)
A hidden `website` field that real users won’t fill but bots might. If it’s filled, the submit handler silently “succeeds” and resets the form so bots don’t learn anything.

### 2) reCAPTCHA v3 (invisible)
We call `grecaptcha.execute(...)` at submit time and include the token in a hidden input named `g-recaptcha-response`. Formspree will verify the token server-side.

**Set it up once:**
1. In the Google reCAPTCHA Admin Console, create a **reCAPTCHA v3** key and add your domain (add `localhost` for local testing).
2. In **Formspree → Your Form → Settings → Spam Protection**, enable reCAPTCHA and paste your **Secret key**.
3. In `index.html`, replace `YOUR_RECAPTCHA_SITE_KEY` with your **Site key** (it appears in the script tag and the `grecaptcha.execute()` call).
4. Publish. Tokens are generated at the moment of submit and expire quickly; that’s expected.

> Prefer a visible checkbox? You can switch to reCAPTCHA v2, or to hCaptcha if you want an alternative provider. The JS is modular enough to swap.

---

## Accessibility notes

- All form inputs have `<label>` elements.
- The success/error status bar uses `aria-live="polite"` so screen readers announce updates.
- The selected date window field is **read-only** and populated automatically when a visitor clicks a date card.

---

## Customizing look & feel

- **Title & copy:** Edit the `<title>`, `<h1>`, and intro paragraph near the top.
- **Hero image:** Replace `HH-pano.jpg` with your own (keep the same filename or update the `src`).
- **Colors & fonts:** Styles live in the `<style>` tag. You can change the button color, background, or import a Google Font.
- **Favicons & social:** Add a favicon and Open Graph/Twitter meta tags for a more polished share preview.

---

## Troubleshooting

- **Form not sending?** Double-check the Formspree endpoint, and verify the form in the Formspree dashboard. If using reCAPTCHA v3, confirm the domain is added and the **Secret key** is set in Formspree.
- **reCAPTCHA message shows “could not load”?** The script might be blocked by an extension or the site key/domain doesn’t match. Reload and confirm your keys.
- **Image not showing?** Ensure `HH-pano.jpg` is in the same folder as `index.html` (or update the `src` path).

---

## Roadmap / nice-to-haves

- Add an inline “Thank you” card with a mini confetti animation after successful submit.
- Extract CSS to its own file if the page grows.
- Optional analytics (e.g., Plausible) to count visits and submissions.
- CI to validate HTML and run link checks on PRs.

---

## Changelog

**2025‑09‑12**
- Fixed June window to **June 6 – July 8, 2026** and unified the button value.
- Added **honeypot** field and **labels** for accessibility.
- Replaced browser alerts with an **inline status bar**.
- Integrated **reCAPTCHA v3 (invisible)**; added setup steps in this README.
