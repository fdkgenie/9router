# Checklist for Amp CLI Implementation in 9router

1. [ ] Add backend helper to call Amp login endpoint and return auth URL.
2. [ ] Implement `POST /api/amp-cli/login` (returns `authUrl`, `verificationCode`, `expiresAt`).
3. [ ] Extend `POST /api/cli-tools/amp-settings` to write `settings.json` + `secrets.json` files.
4. [ ] Ensure `DELETE /api/cli-tools/amp-settings` removes saved Amp url/key and resets stored states.
5. [ ] Update `AmpToolCard` to trigger login, open browser, and display connected status.
6. [ ] Add manual config snippet showing both Amp config files and optional env vars.
7. [ ] Ensure combo definitions (e.g., `amp-gemini`) map to actual provider/model sequences.
8. [ ] Log actions/responses for each API endpoint (success + error cases).
9. [ ] Write integration tests mocking filesystem writes for the new endpoints.
10. [ ] Document CLI workflow in README/docs for future reference.
