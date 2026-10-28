# Benvela Health website

The one-page site for **https://benvelahealth.com**, so Apple can verify the company during Developer Program enrollment. It is plain HTML and CSS: no frameworks, no build step.

- `index.html`: the home page (name, one-sentence description, "Coming soon to the App Store", contact, footer)
- `privacy.html`: a short placeholder privacy policy (the app's full policy comes later)
- `style.css`: the look (the app's green accent, neutral everything else, light and dark mode)
- `CNAME`: tells GitHub the site lives at `benvelahealth.com`. Do not delete it.
- `../.github/workflows/pages.yml`: publishes this folder to GitHub Pages whenever it changes on `main`

To preview locally: `cd website && python3 -m http.server 8000`, then open http://localhost:8000.

## Why a separate repository?

The app code lives in a private repository. GitHub Pages is free only for public repositories, so the website has its own small public one. Never put app code, secrets or health documents in this repository.

## Steps in GitHub (about 5 minutes)

1. Open github.com/kendradom6/benvelahealth-site and click **Settings** (top row of tabs).
2. In the left sidebar click **Pages**.
3. Under **Build and deployment**, set **Source** to **GitHub Actions**.
4. Click **Actions** (top row of tabs), then **Website** in the left list, then **Run workflow** (green button, branch `main`). Wait for the green check (about a minute).
5. Go back to **Settings, Pages**. Under **Custom domain** type `benvelahealth.com` and click **Save**. GitHub checks the DNS records (see below). It can say "DNS check in progress" for a while; that is normal.
6. When it turns green, tick **Enforce HTTPS**. (The box stays grey until GitHub has issued the certificate, which can take up to an hour. Come back later if so.)

Do the Porkbun step below first, or right after step 5, so the DNS check can pass.

## DNS records at Porkbun

Log in to Porkbun, go to **Domain Management**, find `benvelahealth.com`, click **Details**, then **DNS Records**.

**Delete** any existing "parking page" records first: Porkbun adds an `ALIAS` record for `benvelahealth.com` and a `CNAME` for `*.benvelahealth.com` pointing at `pixie.porkbun.com`, which would fight with the ones below.

**Do not touch the MX or TXT records.** They make `admin@benvelahealth.com` email forwarding work.

Then add these four **A** records (Host left blank, which means the bare domain), one per row, TTL default:

| Type | Host | Answer |
|---|---|---|
| A | *(blank)* | `185.199.108.153` |
| A | *(blank)* | `185.199.109.153` |
| A | *(blank)* | `185.199.110.153` |
| A | *(blank)* | `185.199.111.153` |

And this **CNAME** so that `www.benvelahealth.com` also works (GitHub redirects it to the bare domain):

| Type | Host | Answer |
|---|---|---|
| CNAME | `www` | `kendradom6.github.io` |

DNS changes usually take a few minutes but can take a few hours. To check, open https://benvelahealth.com in a browser; you should see the Benvela Health page with a padlock.

## When it is live

Use `https://benvelahealth.com` as the website in Apple's Developer Program enrollment and `https://benvelahealth.com/privacy.html` as the privacy policy URL in App Store Connect. Apple will also check that the contact email (`admin@benvelahealth.com`) works.

## Changing the site later

Edit the files in this folder and merge to `main`. The workflow republishes automatically.
