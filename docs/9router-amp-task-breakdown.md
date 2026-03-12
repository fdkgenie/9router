# Task Breakdown for Amp CLI Job in 9router

## A. Backend API Tasks
1. **amp login helper** (lib/amp/login.js)
   - POST to `https://ampcode.com/api/login` with API key, handle response, return `authUrl` + `verificationCode`.
2. **Amp login endpoint** (`src/app/api/amp-cli-login/route.js`)
   - Accept API key, call helper, return JSON with login info.
3. **Amp settings endpoint updates** (`src/app/api/cli-tools/amp-settings/route.js`)
   - POST: create directories and write JSON files.
   - DELETE: remove Amp config or clear entries.
   - Update internal settings/DB with `ampModelMappings`.
4. **Store settings/secrets** using `fs-extra` (async) with merge semantics.
5. **Logging + error responses** for each endpoint.

## B. Frontend/UI Tasks
1. **Add "Connect Amp CLI" button** to `AmpToolCard` that:
   - Calls `/api/amp-cli/login`, opens returned `authUrl`.
   - After login, POST to `/api/cli-tools/amp-settings` to persist config.
2. **Display connected status** once `amp.url` is present.
3. **Show manual config instructions** for `settings.json` & `secrets.json` paths.
4. **Expose Amp combo mapping** with fallback lists in `AmpToolCard`.

## C. Combo + Provider Configuration
1. Define `amp` combo entry referencing actual provider/model chain.
2. Ensure fallback logic (existing combo handler) handles Amp CLI `model` alias.
3. Document addition in README if necessary.

## D. Testing & Documentation
1. **Unit tests** for backend endpoints (mock fs & fetch).
2. **Integration tests** for Amp endpoints using temporary directories.
3. Update documentation with new Amp CLI workflow (README or docs page).
4. Optional e2e script template replicating `amp-cli-e2e.sh` behavior.
