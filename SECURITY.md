# Security Policy

The weather widget handles no accounts, no credentials and no personal data. If you find a security issue, report it privately.

## Reporting a vulnerability

**Do not open a public issue.** Use GitHub's private vulnerability reporting (repository **Settings** → **Security** → **Private vulnerability reporting**).

We aim to reply within 72 hours and to release a fix as soon as practical.

## Scope

**It runs inside Hermes Desktop** (the Electron renderer). There is no backend, no network server, and no storage beyond `ctx.storage`, the plugin-scoped key-value store in your Hermes profile.

**It ships no secrets**: no API keys, no tokens, no accounts of any kind.

## Data flow and hosts

Everything that leaves your machine, and the five hosts it goes to.

| Direction | What | When |
|---|---|---|
| out | The city name, used to resolve coordinates: typed by you, or derived from your public IP while auto-location is on | when you set or switch a location, and whenever auto-location resolves a city |
| out | Coordinates (lat/lon) of the resolved location, and the requested date range | every weather refresh |
| out | Your public IP | **only** while auto-location is on, via ipwho.is |
| out | Nothing else | - |

The hosts, all free, keyless and CORS-enabled:

- `api.open-meteo.com`: the forecast
- `archive-api.open-meteo.com`: the historical archive
- `air-quality-api.open-meteo.com`: air quality
- `geocoding-api.open-meteo.com`: the city search
- `ipwho.is`: IP geolocation, called only while auto-location is on

**No telemetry**, no analytics, no crash reporting, and no endpoints beyond those five.

**Every request** sends `credentials: 'omit'` and `referrerPolicy: 'no-referrer'`, with one timeout covering the whole call (headers and body).

## Auto-location (IP)

**Off by default**, opt-in. While it is on, the widget calls ipwho.is with your public IP. The `auto · on` badge tooltip says so. Turn auto-location off to stop every IP lookup.

## Antivirus false positives

Windows Defender's machine-learning heuristic occasionally flags readable, unminified JavaScript like this bundle. **It is a false positive**: there is nothing malicious in the file.

**Verify it in one command.** `Get-FileHash .\desktop\plugin.js -Algorithm SHA256` must equal the SHA256 in this tag's release notes.

**If it is blocked anyway.** Smart App Control is refusing an unsigned file. Turn it off for the install and back on afterwards: **Windows Security** → **App & browser control** → **Smart App Control settings**. Turning it back on needs the April 2026 update (build 26100.8246 or later); on earlier Windows 11 it cannot be switched back on without resetting the PC. See the [Smart App Control FAQ](https://support.microsoft.com/en-us/windows/security/threat-malware-protection/smart-app-control-frequently-asked-questions).
