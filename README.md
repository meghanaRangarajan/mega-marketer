# MEGa Marketing — static site (GitHub Pages)

Plain HTML. No build step. Every folder is a URL.

## How the URLs work (the important part)

With GitHub Pages, **the folder structure is the URL structure.** You do not need subdomains.

```
index.html                →  mega-marketer.com/
ai-visibility/index.html  →  mega-marketer.com/ai-visibility/
about/index.html          →  mega-marketer.com/about/        (add later)
insights/index.html       →  mega-marketer.com/insights/     (add later)
```

So the piece that used to live at `ai-visibility.mega-marketer.com` now lives at
`mega-marketer.com/ai-visibility/`, which is exactly the format you wanted.

To publish a new article: make a new folder with an `index.html` inside it
(for example `ai-advertising/index.html`), and it is automatically live at
`mega-marketer.com/ai-advertising/`. Copy `ai-visibility/index.html` as your starting template.

## One-time deploy

1. Create a new GitHub repo, e.g. `mega-marketer` (public).
2. Put these files at the repo root and push to the `main` branch.
3. Repo → **Settings → Pages**. Source: **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
4. Still under Pages, set **Custom domain** to `mega-marketer.com`. (The `CNAME` file in this repo already does this, but confirm it here.)
5. Tick **Enforce HTTPS** once it becomes available (can take a few hours; the certificate is free and automatic).

Note: because you are using a custom domain, the repo name does **not** appear in the
URL. `mega-marketer.com/ai-visibility/` works even though the repo is called `mega-marketer`.

## DNS records (set these at whoever manages mega-marketer.com's DNS)

For the apex domain `mega-marketer.com`, create four **A** records pointing to:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

(Optional but recommended, four **AAAA** records for IPv6:)

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

For `www`, create one **CNAME** record pointing `www` to `<your-github-username>.github.io`.
GitHub will then redirect www to the apex automatically.

## Retiring the old subdomain

Once `mega-marketer.com/ai-visibility/` is live, retire `ai-visibility.mega-marketer.com`:

- Simplest: delete that subdomain's DNS record so it stops resolving.
- If you want old links to keep working: serve the file in `_old-subdomain-redirect/index.html`
  from that subdomain. It forwards visitors to the new path. (GitHub Pages cannot do a true
  server-side 301, so this is a client-side redirect, which is fine for this purpose.)

Do the same pattern for `ai-advertising.mega-marketer.com` when you migrate that piece.

## Two things GitHub Pages will NOT do (so you know)

1. **The contact form.** GitHub Pages is static only, so the form on the homepage cannot
   submit on its own. Easiest fixes: point it at a free form service like Formspree, or
   replace it with a `mailto:` link / your LinkedIn. (This is the one place Netlify would
   have been simpler — its built-in form handling. You can always switch hosts later without
   changing any of this HTML.)
2. **True server-side redirects.** Old URLs are handled with the small redirect page above
   rather than a real 301.

## Structure in this folder

```
.
├── index.html                     ← homepage
├── ai-visibility/
│   └── index.html                 ← the migrated AI-visibility article (also your template)
├── _old-subdomain-redirect/
│   └── index.html                 ← optional stub to place on the old subdomain
├── CNAME                          ← custom domain for GitHub Pages
└── README.md
```
