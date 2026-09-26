# Google authentication via `gcloud` Application Default Credentials

Working note for M-013 (set up exploratory integration credentials). Records why we moved off the Desktop-OAuth loopback and device-code flows and how to complete authorization with the `gcloud` CLI instead. Promote or fold into the M-021 closeout once validated.

## Why not the earlier approaches

- **Desktop-app loopback flow** (`InstalledAppFlow.run_local_server`) failed repeatedly inside the devcontainer with `MismatchingStateError` and `Address already in use`. The devcontainer's automatic port-forward detection appears to probe newly opened listening sockets, consuming the callback before the real browser redirect arrives. This is a container-networking problem, not a credentials problem.
- **OAuth device-code grant** (`https://oauth2.googleapis.com/device/code`) returned `invalid_scope`. Google's device/limited-input grant only supports a small allowlist of scopes and explicitly rejects Docs/Sheets scopes, regardless of client type. This is a hard platform restriction — no client reconfiguration fixes it.
- **Manual authorization-code paste** (loopback redirect to `http://localhost`, code copied from the failed-to-load address bar) works but requires a fragile one-shot interactive terminal prompt and is easy to break by piping through a heredoc that closes stdin.

## The `gcloud` ADC approach

Run the authorization on the **host machine** (where `gcloud` is installed), not inside the devcontainer terminal. This sidesteps all of the container-networking issues above because the browser redirect completes entirely on the host's own loopback interface.

### Confirmed facts (source: Google Cloud's [Application Default Credentials](https://docs.cloud.google.com/docs/authentication/application-default-credentials) and [`gcloud auth application-default login`](https://docs.cloud.google.com/sdk/gcloud/reference/auth/application-default/login) docs)

- By default, `gcloud auth application-default login` only grants the `https://www.googleapis.com/auth/cloud-platform` scope for user credentials. It does **not** include Docs/Sheets scopes unless you ask for them explicitly.
- To add scopes for services outside core Google Cloud (Docs, Sheets, Drive, etc.), you must supply your **own** OAuth client ID via `--client-id-file` and list the scopes with `--scopes`.
- The resulting credential file is written to a well-known location:
  - Linux/macOS: `$HOME/.config/gcloud/application_default_credentials.json`
  - Windows: `%APPDATA%\gcloud\application_default_credentials.json`
- This file uses the standard `authorized_user` JSON schema (`client_id`, `client_secret`, `refresh_token`, `type: authorized_user`), which is directly loadable by `google.oauth2.credentials.Credentials.from_authorized_user_file()` — the same shape our tooling already expects.

### Steps (run on the host, not in the devcontainer)

1. Confirm the existing OAuth client file is present at `~/.config/cmte-gvrnce/google-oauth-client.json` (the same file bind-mounted into the devcontainer).
2. Run:

   ```bash
   gcloud auth application-default login \
     --client-id-file="$HOME/.config/cmte-gvrnce/google-oauth-client.json" \
     --scopes="https://www.googleapis.com/auth/cloud-platform,https://www.googleapis.com/auth/documents,https://www.googleapis.com/auth/spreadsheets,https://www.googleapis.com/auth/userinfo.email,openid"
   ```

   `gcloud` rejects a custom `--scopes` list that omits `cloud-platform` (confirmed by running the command: `Invalid value for [--scopes]: https://www.googleapis.com/auth/cloud-platform scope is required but not requested`), so it must always be included even though this project doesn't otherwise use core Cloud APIs.

   This opens your default browser for a normal, host-side consent flow. If you need a headless variant, check `gcloud auth application-default login --help` on your installed CLI version for the no-browser flag name before using it — flag names can shift between SDK versions, so confirm locally rather than assuming.
3. After success, copy the ADC file into the directory the devcontainer mounts, so it appears inside the container automatically:

   ```bash
   cp "$HOME/.config/gcloud/application_default_credentials.json" \
      "$HOME/.config/cmte-gvrnce/google-token.json"
   chmod 600 "$HOME/.config/cmte-gvrnce/google-token.json"
   ```

4. Inside the devcontainer, verify the token cache loads:

   ```bash
   python3 -c 'from google.oauth2.credentials import Credentials; c = Credentials.from_authorized_user_file("'"$GOOGLE_TOKEN_CACHE"'"); print("scopes:", c.scopes)'
   ```

## Guardrails

- Never commit `google-oauth-client.json`, `application_default_credentials.json`, or `google-token.json`. They live only under `~/.config/cmte-gvrnce/`, which is gitignored and bind-mounted, not tracked in this repo.
- The ADC credentials created here are distinct from your personal `gcloud` CLI login — they are scoped specifically for this project's Docs/Sheets smoke checks, not for managing Google Cloud resources.
- Record the confirmed scopes and any account/plan constraints in the M-013 evidence log per [exploratory-sprint.md](approach/exploratory-sprint.md), without recording secret values.
