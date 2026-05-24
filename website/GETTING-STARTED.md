# Getting your portfolio live

A start-to-finish runbook, Emma. About 30–60 minutes end to end, mostly waiting on DNS.

---

## 0. One-time Mac setup

Open **Terminal** (Cmd+Space → "Terminal") and run:

```bash
xcode-select --install
```

That installs `git` and a C compiler. Click through the popup.

Install two apps from the web:

- **Cursor** — https://cursor.com (your AI-powered code editor)
- **Node.js LTS** — https://nodejs.org (for the live preview server later)

Then create a free account on:

- **GitHub** — https://github.com
- **Vercel** — https://vercel.com (sign in with your GitHub account — it makes step 4 painless)

---

## 1. Open the project in Cursor

The site lives at `~/Desktop/portfolio photos/website/`. I'd recommend moving it somewhere cleaner first:

```bash
mv ~/Desktop/portfolio\ photos/website ~/Documents/portfolio
```

Then in Cursor: **File → Open Folder…** → pick `~/Documents/portfolio`.

Open `index.html` to see the structure, `style.css` to see the design tokens at the top of the file.

### Vibe-code your edits

Press **Cmd+L** in Cursor to open the AI chat. Try prompts like:

- "Tighten the hero summary so it's two sentences max."
- "Reorder the engagement cards so CSIRO 6G is first."
- "Change the accent color from mint green to a deeper teal — update the CSS tokens."
- "Add a sixth card for an upcoming UNSW guest lecture."
- "Add a publications section after the engagements with my IEEE paper, linked to its IEEE Xplore page."

You can also press **Cmd+K** inside a file to edit a specific block inline.

### Preview as you go

Easiest: install the **Live Server** extension in Cursor's extensions panel, right-click `index.html` → **Open with Live Server**. The page auto-reloads on every save.

Or just double-click `index.html` in Finder to open it in Safari/Chrome, and refresh manually.

### Things you'll likely want to swap in

A few placeholders I couldn't fill from your resume:

- `card__link` URLs currently point to `#`. Replace with real links — IEEE Xplore for the paper, event page or LinkedIn post for each talk.
- LinkedIn URL — I guessed `linkedin.com/in/emma-romanous`. Update if it's different.
- The "Read the paper →" link — paste your IEEE Xplore DOI URL.
- Drop your résumé PDF in the folder as `resume.pdf` and the download link will work.
- Want a phone number on the page? Add a `<li>` to the contact list — I left it off so it's not scraped by bots, but it's your call.

---

## 2. Put it on GitHub

In Cursor's built-in terminal (Terminal → New Terminal, or Ctrl+`):

```bash
cd ~/Documents/portfolio
git init
git add .
git commit -m "first commit"
```

Then on github.com, click **New repository**, name it something like `portfolio` or `emma-romanous`, public or private — both work with Vercel. **Don't** check "add README".

GitHub will show you commands under "push an existing repository". Copy-paste them into Cursor's terminal:

```bash
git remote add origin https://github.com/yourhandle/portfolio.git
git branch -M main
git push -u origin main
```

Reload the GitHub page — your files should be there.

---

## 3. Deploy to Vercel

1. Go to https://vercel.com/new
2. Click **Import** next to your `portfolio` repo. (If you don't see it, click "Adjust GitHub App Permissions" and grant access.)
3. Don't change any settings. Click **Deploy**.
4. ~30 seconds later you'll have a live URL like `portfolio-yourhandle.vercel.app`.

From now on, every `git push` from Cursor triggers an auto-deploy. That's the whole CI/CD.

---

## 4. Point your custom domain

In your Vercel project: **Settings → Domains → Add** → type your domain (e.g. `emmaromanous.com`) → **Add**.

Vercel will show you DNS records to add. The two common cases:

**For the apex domain (`emmaromanous.com`):**
- **Type:** A
- **Name:** @ (or leave blank)
- **Value:** `76.76.21.21`

**For `www.emmaromanous.com`:**
- **Type:** CNAME
- **Name:** www
- **Value:** `cname.vercel-dns.com`

Most people add both and set one to redirect to the other (Vercel handles the redirect for you in the Domains panel).

Log into your registrar (Cloudflare / Porkbun / Namecheap / wherever you bought it), find DNS settings, add the records Vercel showed you, save. Wait 5–30 minutes. Vercel auto-issues an HTTPS cert as soon as DNS resolves.

---

## 5. Keep shipping

The vibe-coding loop forever:

1. Edit in Cursor with AI assist (Cmd+L for chat, Cmd+K for inline).
2. Preview locally with Live Server.
3. `git add . && git commit -m "tweak hero copy" && git push`
4. Vercel redeploys, your domain reflects it in ~30 seconds.

That's it. You have a real, hosted, version-controlled personal site.

---

## Quick reference

```bash
# Save your work
git add .
git commit -m "describe what you changed"
git push

# See what's changed since last commit
git status
git diff

# Undo uncommitted changes to a file
git checkout -- filename
```

If anything breaks, paste the error into Cursor's AI chat (Cmd+L) — it's surprisingly good at unblocking git, DNS, or CSS weirdness.
