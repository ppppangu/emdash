# @emdash-cms/registry-cli

## 0.0.1

### Patch Changes

- [#929](https://github.com/emdash-cms/emdash/pull/929) [`5464b55`](https://github.com/emdash-cms/emdash/commit/5464b551f0100d33fe9adbdd74d3444d37321209) Thanks [@ascorbic](https://github.com/ascorbic)! - Fixes the CLI hanging indefinitely after a successful `login` or `logout`. `run()` was returning correctly, but something in the OAuth path left a ref'd handle alive that prevented Node's event loop from draining. Workaround: force-exit at the top level once `runMain` resolves. The underlying handle leak is unidentified.

- [#929](https://github.com/emdash-cms/emdash/pull/929) [`5464b55`](https://github.com/emdash-cms/emdash/commit/5464b551f0100d33fe9adbdd74d3444d37321209) Thanks [@ascorbic](https://github.com/ascorbic)! - Switches the login flow to request granular OAuth scopes derived from the `@emdash-cms/registry-lexicons` lexicon set instead of the broad `transition:generic`: `repo:` for every record-shaped lexicon (package profile, package release, publisher profile, publisher verification) and `rpc:<nsid>?aud=*` for every aggregator query (`getLatestRelease`, `getPackage`, `listReleases`, `resolvePackage`, `searchPackages`). Display name resolution no longer goes through `com.atproto.server.getSession`; the handle is read from the DID document via `LocalActorResolver` so the CLI doesn't need an `rpc:com.atproto.*` scope and isn't affected by PDS-side DPoP/Bearer compatibility quirks. If the PDS rejects the granular scopes with `invalid_scope`, login automatically retries once with `transition:generic` and prints a notice. Existing sessions continue working with their original scope until they're revoked or re-issued.

- [#929](https://github.com/emdash-cms/emdash/pull/929) [`5464b55`](https://github.com/emdash-cms/emdash/commit/5464b551f0100d33fe9adbdd74d3444d37321209) Thanks [@ascorbic](https://github.com/ascorbic)! - Improves `login` error reporting for OAuth response failures. Previously, transient PDS errors surfaced as a bare `unknown_error` with a stack trace; the CLI now prints the HTTP status, endpoint, OAuth error code/description, a body snippet when the response wasn't OAuth-shaped JSON, and a hint to retry on 5xx responses.

- [#923](https://github.com/emdash-cms/emdash/pull/923) [`943df46`](https://github.com/emdash-cms/emdash/commit/943df46d62043df386eef4664fbba4710be16c31) Thanks [@ascorbic](https://github.com/ascorbic)! - Adds `@emdash-cms/registry-cli`: standalone CLI for the experimental plugin registry. Subcommands for `login`, `logout`, `whoami`, `switch`, `search`, `info`, `bundle`, and `publish`. Atproto OAuth via loopback callback server. The `publish` flow fetches the tarball from the URL, verifies a sha256 multihash, extracts and validates `manifest.json`, locally validates each lexicon record, and atomically writes profile + release records (with the EmDash declaredAccess trust extension) via a single atproto `applyWrites`. Distributes via `npx @emdash-cms/registry-cli` to keep atproto deps out of the core CMS install.

- Updated dependencies [[`943df46`](https://github.com/emdash-cms/emdash/commit/943df46d62043df386eef4664fbba4710be16c31), [`943df46`](https://github.com/emdash-cms/emdash/commit/943df46d62043df386eef4664fbba4710be16c31), [`5464b55`](https://github.com/emdash-cms/emdash/commit/5464b551f0100d33fe9adbdd74d3444d37321209), [`943df46`](https://github.com/emdash-cms/emdash/commit/943df46d62043df386eef4664fbba4710be16c31)]:
  - @emdash-cms/plugin-types@0.0.1
  - @emdash-cms/registry-client@0.0.1
  - @emdash-cms/registry-lexicons@0.1.0
