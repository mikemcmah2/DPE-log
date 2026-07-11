# DPE Log

A private, offline-first checkride log for tracking applicants, results, and payments — built as a single self-contained `index.html` file. All data is encrypted in your browser with a passphrase you choose and stored only in that browser's `localStorage`. Nothing is ever sent to a server.

## What it tracks

- **Entries:** date, time, applicant name, school, test type, result (Pass/Fail/Discontinued/Retest), amount paid, form of payment
- **Money collected:** today, this week, this month, this year, all time
- **Pass rate:** a gauge-style chart (green/red, Pass vs. Fail only) with a This Week / This Month / This Year / All Time selector
- **Breakdown table:** pass rate for "All Tests" and each individual test type, across all four time periods
- Editable dropdown lists for School, Test Type, and Payment Type (pre-loaded with your usual values, plus an "add new" button)

## Security model

- On first launch, you set a passphrase. That passphrase is run through PBKDF2 (250,000 iterations) to derive an AES-256-GCM key.
- Every save encrypts your entire log and writes only the encrypted blob to `localStorage`. The passphrase itself is never stored anywhere.
- The code that does this (and how it does it) is fully visible in `index.html` — that's fine. Security here comes from your secret passphrase, not from hiding the method.
- **If you forget your passphrase, there is no way to recover your data.** There's no backdoor, by design.
- This is real device-local storage in your browser — different from an in-chat Claude artifact, which uses Anthropic's cloud-backed storage instead.

## Limitations, honestly

- Data lives in one browser, on one device. Clear your browser data / site data for this page, and it's gone — **use Export Backup regularly.**
- This protects data at rest from casual access. It does not protect against a compromised browser, a keylogger, or someone who has your passphrase.
- Switching browsers or devices means starting fresh unless you use Export on the old one and Import on the new one.

## Deploying to GitHub Pages

1. Create a new repository on GitHub (can be public — the code being public doesn't expose your data).
2. Upload **all files in this folder** to the repo root — `index.html`, `README.md`, `favicon.ico`, `favicon-16.png`, `favicon-32.png`, `apple-touch-icon.png`, `icon-192.png`, `icon-512.png`, and `manifest.webmanifest`. They need to sit alongside `index.html`, not in a subfolder, since the app links to them by relative path.
3. In the repo, go to **Settings → Pages**.
4. Under "Build and deployment," set **Source** to "Deploy from a branch," pick your default branch (usually `main`) and `/ (root)`, then **Save**.
5. GitHub will give you a URL like `https://yourusername.github.io/your-repo-name/` — that's your app. It can take a minute or two to go live the first time.
6. Open that URL on your phone, then use your browser's "Add to Home Screen" (Safari: Share → Add to Home Screen; Chrome: menu → Add to Home Screen). It'll use the gauge icon and open full-screen, no browser bar.


## Backing up your data

Use the **Export Backup** button (bottom of the app) any time to download a JSON file containing your encrypted data. Keep it somewhere safe (cloud drive, email to yourself, etc.) — it's encrypted, so it's safe to store even in a non-private location, as long as you remember the passphrase. Use **Import Backup** to restore it (on this device or a new one).

## Changing your passphrase

Use the **Change Passphrase** button in the footer. You'll need to already be unlocked to do this.
