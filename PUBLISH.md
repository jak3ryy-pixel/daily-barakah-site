# Putting the Daily Barakah site online

This folder holds the three public pages the app links to:

| Address | File |
|---|---|
| https://dailybarakah.amos.live/ | `index.html` (Help & Support) |
| https://dailybarakah.amos.live/privacy/ | `privacy/index.html` |
| https://dailybarakah.amos.live/terms/ | `terms/index.html` |

It is set up exactly like the Daily Blessing site (`Website/`, which is the repo
`jak3ryy-pixel/daily-blessing-site`): a public GitHub repo, served free by GitHub Pages,
with one DNS record in Cloudflare. Follow the six steps below in order. The whole thing
takes about 15 minutes, plus a wait for the certificate.

Nothing here touches the app's own code, which stays in the private repo.

## Before you start

Look at the pages once on your Mac. In Terminal, run:

```bash
python3 -m http.server 8766 --directory "/Users/jakeryan/Desktop/Daily Blessing/Website-DailyBarakah"
```

then open http://localhost:8766 in Safari. Press Control‑C in Terminal when you are done.
If you change any wording, also change the "Effective" date at the top of the Privacy
Policy and Terms of Use pages.

## Step 1. Create a new public repo on GitHub

1. Go to https://github.com/new (sign in as **jak3ryy-pixel** if asked).
2. Repository name: `daily-barakah-site`
3. Description: `Support, privacy and terms pages for the Daily Barakah iPhone app`
4. Choose **Public**. (GitHub Pages on a free account needs a public repo. The pages
   are public anyway; there is nothing private in this folder.)
5. Leave every "Initialize this repository" box **unticked**. No README, no .gitignore,
   no license. The folder already has everything.
6. Click **Create repository**. Leave the page open; you will not need to copy anything
   from it, but it shows the repo exists.

## Step 2. Push this folder to that repo

Open Terminal and paste these commands one at a time, pressing Return after each.

```bash
cd "/Users/jakeryan/Desktop/Daily Blessing/Website-DailyBarakah"
```

```bash
git init -b main
```

```bash
git add -A
```

```bash
git commit -m "Support, privacy and terms pages for Daily Barakah"
```

```bash
git remote add origin https://github.com/jak3ryy-pixel/daily-barakah-site.git
```

```bash
git push -u origin main
```

If the last command asks you to sign in, use the same GitHub login that worked for the
Daily Blessing site. When it finishes, refresh the GitHub page from Step 1: you should see
`index.html`, `style.css`, `privacy`, `terms`, `CNAME` and the two icon files.

From now on, to change the site: edit the files in this folder, then in Terminal run
`git add -A`, `git commit -m "what you changed"`, `git push`. The site updates within a
minute or two.

## Step 3. Turn on GitHub Pages

1. On the repo page, click **Settings** (the tab on the right, with the gear).
2. In the left column, click **Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
4. Under **Branch**, choose **main** and leave the folder as **/ (root)**. Click **Save**.
5. Wait a minute, then refresh. A box at the top says "Your site is live at
   https://jak3ryy-pixel.github.io/daily-barakah-site/". Do not worry if the address
   looks odd; the next two steps give it the real name.

## Step 4. Add the custom domain in GitHub

Still on the **Pages** settings page:

1. Under **Custom domain**, type `dailybarakah.amos.live` and click **Save**.
2. GitHub shows "DNS check in progress" or an error. That is expected, because the
   Cloudflare record from Step 5 does not exist yet. Go and do Step 5, then come back.

(The folder already contains a `CNAME` file with this name. GitHub may make a small
commit to the repo when you save the domain. If it does, run `git pull` in this folder
the next time you edit the site, so your copy matches GitHub's.)

## Step 5. Add the DNS record in Cloudflare

This is the same kind of record that Daily Blessing's README describes for
`dailyblessing.amos.live`. Only the name is different.

1. Sign in at https://dash.cloudflare.com and open the **amos.live** domain.
2. In the left column, click **DNS**, then **Records**.
3. Click **Add record** and fill it in exactly like this:

   | Type | Name | Target | Proxy status | TTL |
   |---|---|---|---|---|
   | CNAME | `dailybarakah` | `jak3ryy-pixel.github.io` | **DNS only** (grey cloud) | Auto |

   The proxy switch must be **off** (the cloud icon grey, not orange). If it is orange,
   GitHub cannot issue the HTTPS certificate and the site will show a security warning.
4. Click **Save**.

Just type `dailybarakah` in the Name box, not the full address; Cloudflare adds
`.amos.live` itself.

## Step 6. Turn on HTTPS and check

1. Back in GitHub, **Settings → Pages**. Within a few minutes the DNS check turns green.
2. Tick **Enforce HTTPS**. If the box is greyed out, GitHub is still getting the
   certificate. Wait 10 to 30 minutes and refresh. If it still says "certificate: none"
   after an hour, remove the custom domain, click Save, add it back and Save again; that
   restarts the request (it worked instantly for Daily Blessing). Then run `git pull` in
   this folder, because GitHub commits the CNAME removal and re‑add.
3. Open all three on your iPhone and check the padlock shows:
   - https://dailybarakah.amos.live/
   - https://dailybarakah.amos.live/privacy/
   - https://dailybarakah.amos.live/terms/

## Afterwards

- These three addresses are already in the app
  (`Apps/DailyBarakah/Sources/Theme/DailyBarakahTheme.swift`), on the paywall and in
  Settings. Nothing to change there.
- In App Store Connect, use `https://dailybarakah.amos.live/privacy/` as the Privacy
  Policy URL and `https://dailybarakah.amos.live/` as the Support URL, and link the Terms
  page in the app description, as for Daily Blessing.
- The certificate renews by itself. The pages are dated October 19, 2026; update the date
  whenever the wording changes.
