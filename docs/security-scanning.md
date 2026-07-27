# Security scanning

PagePith uses three dependency and source-code checks:

- `npm audit --omit=dev` checks production dependencies against npm advisories.
- `npm run test:snyk:dependencies` runs Snyk Open Source across detected package manifests.
- `npm run test:snyk:code` runs Snyk Code static analysis over the source tree.

Both Snyk scans fail on high- or critical-severity findings. `npm run test:snyk` runs both scans, and `npm run check` runs unit tests, the production build, and both Snyk scans.

## One-time Snyk setup

Snyk requires an account and authentication. Do not put a token in this repository or a shell-history command.

For local scans, authenticate through the CLI:

```sh
npx snyk auth
npm run test:snyk
```

For GitHub Actions, set the repository secret from an interactive prompt:

```sh
gh secret set SNYK_TOKEN --repo heyjdev/pagepith
```

Paste the token only when `gh` prompts for it. The CI workflow reads it as `${{ secrets.SNYK_TOKEN }}`.

Snyk Code must also be enabled for the Snyk organization associated with the token. Forked pull requests intentionally skip the Snyk job because GitHub does not expose repository secrets to untrusted forks; the scans run after trusted code reaches a repository branch.

## Commands

```sh
npm test                         # unit tests only; no Snyk account required
npm run test:snyk:dependencies  # dependency scan
npm run test:snyk:code          # static source scan
npm run test:snyk               # both Snyk scans
npm run check                    # unit tests, build, and both Snyk scans
```
