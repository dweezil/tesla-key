# tesla-key

Hosts the **public** key for the Tesla Fleet API, used by the Home Assistant
Tesla Fleet integration. Tesla fetches it from:

https://tesla.dernmonkeys.com/.well-known/appspecific/com.tesla.3p.public-key.pem

File in this repo: `.well-known/appspecific/com.tesla.3p.public-key.pem`

## Security

- This is a **public** key (EC prime256v1). It is meant to be published; anyone can read it.
- The matching **private** key stays in Home Assistant and must never be committed here.
- Until setup is done the file holds a placeholder line, not a key.

## Hosting

- GitHub Pages, branch `main`, root folder, custom domain `tesla.dernmonkeys.com` (`CNAME` file).
- `.nojekyll` is required: without it Jekyll skips dot-folders and `.well-known` would 404.
- DNS at Namecheap (dernmonkeys.com > Advanced DNS):

  | Type  | Host  | Value                |
  |-------|-------|----------------------|
  | CNAME | tesla | dweezil.github.io.   |

- After DNS resolves and GitHub issues the certificate, enable Settings > Pages > **Enforce HTTPS**.
- Domain should be verified at the account level (GitHub Settings > Pages > Verified domains,
  TXT record `_github-pages-challenge-dweezil.dernmonkeys.com`) to prevent subdomain takeover.

## Replacing the key

1. Get the public key PEM from the Home Assistant Tesla Fleet setup
   (`-----BEGIN PUBLIC KEY-----` ... `-----END PUBLIC KEY-----`). Never use the private key.
2. Overwrite `.well-known/appspecific/com.tesla.3p.public-key.pem` with it (whole file, LF endings).
3. Commit and push to `main`; wait for the Pages build (1-2 min).
4. Check: `curl https://tesla.dernmonkeys.com/.well-known/appspecific/com.tesla.3p.public-key.pem`
   returns the PEM, then continue the Tesla developer / Home Assistant registration.
