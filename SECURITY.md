# Security

## Reporting a vulnerability

Please report anything you find privately, by opening a
[GitHub security advisory](https://github.com/wizzardpig-stack/astra-/security/advisories/new)
on this repository. Please don't open a public issue for a security problem.

Include what you did, what happened, and which browser and version you used.
Expect a first reply within about a week.

## How this application is built

ASTRA//SIGNATURE is a static, single-page browser application served from
GitHub Pages. That shapes its whole threat model:

- **No backend, no database, no accounts.** There is no server-side code to
  attack and no stored user data to breach.
- **No network requests at runtime.** The page fetches nothing after load. The
  Content Security Policy sets `connect-src 'none'`, so birth data has nowhere
  to go even if something were injected into the page.
- **Birth data never leaves the browser.** Name, date, time and coordinates
  live in memory for the life of the tab. They are never transmitted and never
  written to storage. `form-action 'none'` also stops them from ever reaching a
  URL query string.
- **Only display preferences are persisted.** `localStorage` holds the chosen
  palette, lens and view, and nothing else.
- **One third-party origin.** Web fonts load from Google Fonts
  (`fonts.googleapis.com`, `fonts.gstatic.com`). Those two origins are the only
  ones the CSP permits, the page sends `referrer: no-referrer`, and the
  interface degrades to system fonts if they fail to load.

## Content Security Policy

The policy is declared in a `<meta http-equiv>` in `index.html`, because GitHub
Pages serves static files and cannot set response headers.

`script-src` and `style-src` include `'unsafe-inline'`: the application is a
deliberately self-contained single file, so its script and stylesheet are
inline. Everything else is locked down — `default-src 'self'`, `object-src`,
`base-uri`, `form-action` and `connect-src` are all `'none'` or `'self'`.

### Known limitation: clickjacking

`frame-ancestors` cannot be enforced from a `<meta>` policy, and GitHub Pages
cannot send `X-Frame-Options`. A third party can therefore embed the page in an
iframe. The practical impact is low — there is no session, no authentication
and no state-changing action to hijack — but it is a real gap. Serving the site
from a host that can set response headers would close it.

## Handling of user input

Astronomical output is written with `textContent`, and the one place a
user-supplied name reaches generated markup (the exported signature SVG) passes
it through `escapeXml()` first. There is no `eval`, no `new Function`, no
`document.write`, and no reading of URL parameters or `postMessage`, so there is
no attacker-controlled path into the page.
