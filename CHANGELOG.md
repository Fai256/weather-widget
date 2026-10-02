# Changelog

## v2.0 (2026-10-02)

The 2.x line opens: the widget moves out of the titlebar into the status bar. Nothing to do on upgrade - your saved locations, units, section state and show/hide choice all carry over.

### Changed

- **Packaging**: `plugin.yaml` plus the bundle at `desktop/plugin.js`.
- **Home**: the status bar, left edge of the right cluster, with the popover opening upward.
- **Show or hide**: from the bar's right-click menu under **Show in status bar** → **Weather**; the choice persists across restarts.
- **The forecast**: renders all 7 rows the request already asked for.
- **The historical charts**: the weekly view draws the past 7 days from the archive, like the 30-day and 12-month views, instead of the coming week.
- **The hourly strip**: hovering an hour names that hour's condition, the way the forecast rows already did.
- **The icons follow the clock**: no sun glyph survives the night. Clear and mostly clear show the moon, partly cloudy shows the plain cloud (Unicode has no moon-behind-cloud), and drizzle, light rain and light showers swap their sun-behind-cloud glyph for the plain rain cloud.
- **A missing precipitation value**: a day with none shows a bare `-` instead of `-mm` and `-%`.
- **Auto-location**: stays opt-in, off by default.

### Fixed

- **Air quality**: the 101-150 band reads "Unhealthy for Sensitive Groups"; it shared "Unhealthy" with 151-200.
- **Sunrise, sunset and UV**: read today's row.
- **Archive gaps**: the archive lags a few days behind, so gaps are skipped instead of drawn as 0 °C, and the yearly view averages only real values, capped at 12 month buckets.
- **Section state**: an expanded section no longer reopens collapsed.
- **A typed city**: saves it now (up to 3, newest first), and a single saved location is visible (the Saved row used to need two entries before it rendered at all).
- **The location box**: bounded to 64 characters; whitespace-only saved locations are dropped; rain-window times are zero-padded.
- **Rain windows**: a window now needs real rain, so a drizzle-only hour no longer opens one. A single dry hour no longer splits a shower in two. A window ends when the rain stops, and the icon and tooltip show the hour with the most rain.

### Hardening

- **API calls**: a bad or missing coordinate is rejected before it reaches a host, requests carry no cookies and no referring page (so the weather services cannot tie a lookup to you or your session), and one timeout covers the whole response rather than just the headers.
- **Storage reads**: wrapped during registration, so a storage failure no longer prevents the plugin from loading.

---

## v1.0.1 (2026-09-01)

### Fixed
- Location not found: show a cleaner "Not found - try a different city" message (the place is no longer repeated).

---

## v1.0.0 (2026-08-28)

First public release.

### Features
- Titlebar widget: current temperature + condition icon, click to open
- Popover with 4 collapsible sections: Current, Hourly (24h scroll), Forecast (7d), Historical charts (7d/30d/12m)
- Auto-location via IP (opt-in, ipwho.is) or manual city entry, up to 3 saved locations
- °C/°F + km/h/mph toggle, theme-aware colors, zero dependencies, hand-drawn inline SVG charts
