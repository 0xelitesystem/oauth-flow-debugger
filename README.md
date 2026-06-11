# OAuth Flow Debugger

Paste an OAuth URL, see what's in it, and get warnings about common mistakes. All decoding happens in your browser.

**Live demo:** https://0xelitesystem.github.io/oauth-flow-debugger/

## Use

Open [`index.html`](./index.html). Paste an OAuth-related URL into the input. Click Decode.

The tool detects what kind of URL you pasted, breaks down the components and parameters, and runs an audit looking for common mistakes:

- HTTPS missing
- `client_secret` in URL (critical leak)
- Missing or weak `state` parameter
- PKCE issues (no challenge, plain method instead of S256)
- Implicit flow usage (deprecated)
- Tokens in query strings (leak vector)
- Redirect URI problems

## What URLs work

- Authorization endpoint URLs (`https://provider.com/oauth/authorize?client_id=...`)
- Authorization code callback URLs (`https://yourapp.com/callback?code=...&state=...`)
- Implicit-flow callback URLs (token in fragment: `...#access_token=...`)
- Token endpoint URLs
- OIDC discovery URLs
- Error responses

## Why this exists

OAuth integrations break in confusing ways. Often the URL is the only thing visible, the rest is opaque server-side state. Pasting the URL into a tool that knows the spec catches most issues in 10 seconds.

This is also useful for:

- Reviewing OAuth integrations during code review
- Teaching OAuth concepts (the labeled parameter notes are a quick reference)
- Catching security mistakes before production (tokens in query strings, weak state, PKCE issues)

## Privacy

Everything runs in your browser. Pasted URLs are not sent anywhere. Verify by viewing the source, there are no fetch or XMLHttpRequest calls in the script.

That said: **don't paste production callback URLs containing real authorization codes or tokens** into any web tool. Use it for inspecting the structure of URLs from your dev environment, sample URLs from documentation, or URLs you've already invalidated.

## What this is NOT

- Not a JWT decoder. If a parameter contains a JWT (e.g. `id_token`), use [jwt-inspector](https://github.com/0xelitesystem/jwt-inspector).
- Not a token verifier. Doesn't check signatures.
- Not a security scanner. Catches common OAuth mistakes, not novel attacks against specific providers.
- Not a network tester. Doesn't make any requests; only parses what you paste.

## Run locally

```bash
git clone https://github.com/0xelitesystem/oauth-flow-debugger
cd oauth-flow-debugger
# Open index.html
```

## Contribute

PRs welcome:

- More provider-specific param notes (Auth0, Okta, Azure AD, Google, etc.)
- Additional audit checks
- UI improvements (no frameworks)
- Translations

The audit logic is in `index.html`. Each check is a few lines, easy to add.

## Build

There is no build. Single HTML file.

## License

MIT.

## Related

- [jwt-inspector](https://github.com/0xelitesystem/jwt-inspector), decode JWTs found in OAuth responses
- [webhook-signature-verifier](https://github.com/0xelitesystem/webhook-signature-verifier), verify webhook signatures
- [byok-security-checklist](https://github.com/0xelitesystem/byok-security-checklist), security checklist for BYOK products
