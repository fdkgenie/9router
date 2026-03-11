# TSD: Automate Amp CLI Integration in 9router

## 1. Purpose
Enable 9router to configure Amp CLI end-to-end so users can authenticate through the dashboard and send CLI traffic through existing multi-provider routing/fallbacks without relying on ramclouds-proxy or manual shell edits.

## 2. Scope
- Backend APIs to generate Amp login URLs and persist Amp CLI config.
- File operations on Amp settings/secrets under `~/.config/amp` and `~/.local/share/amp`.
- Dashboard CLI Tools UI adjustments: connect button, status update, manual config hints.
- Use existing combo/fallback infrastructure for CLI models.
- No shell-profile edits; rely solely on Amp config files.
- Logging and errors for visibility.

## 3. Requirements
1. `POST /api/amp-cli/login` returns Amp auth URL, expiration, verification code.
2. `POST /api/cli-tools/amp-settings` writes settings/secrets and updates stored mappings.
3. `DELETE /api/cli-tools/amp-settings` removes saved config and resets Amp combo state.
4. CLI Tool UI triggers login, opens browser, and saves returned URL.
5. Amp combos map to models already supported by router fallback.
6. UI displays sample config snippets (settings + secrets + `AMP_URL`/`AMP_API_KEY`).
7. All file writes handle missing directories and backup existing values.
8. Logging includes success/failure per endpoint.
9. No MITM or subscription bypass logic; upstream Amp validation remains standard.

## 4. Non-Functional
- Cross-platform file paths.
- Use asynchronous fs with error handling.
- Maintain existing security (apiKey guard, optional localhost restriction).
- Provide helpful error messages via JSON responses.

## 5. Acceptance Criteria
- Amp CLI config endpoint passes `curl` with `POST /api/cli-tools/amp-settings` (1) writing settings + secrets and (2) returning `{ ok: true }`.
- CLI tools UI shows "Connected" status after storing new `amp.url`.
- Amp login endpoint returns `authUrl` and instructions.
- Running `amp` CLI (targeting `http://localhost:20127`) works using stored config.
- Existing combos automatically use Amp provider chain.
- Tests cover endpoints with mocks for fs operations.

## 6. Risks & Mitigation
- **Missing directories**: create recursively before writing.
- **Existing secrets overwritten**: merge and preserve other keys when writing `secrets.json`.
- **Amp CLI not installed**: `GET /api/cli-tools/amp-settings` should fail gracefully.
- **Multiple shells**: avoid shell profile edits.
- **Cross-platform**: compute home via `os.homedir()` and use path.join.
