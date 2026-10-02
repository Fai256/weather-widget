# Contributing to Weather Widget

Thanks for taking a look. This is one small plugin, so the best contributions are small too: one change, tested in the app, explained in the pull request.

## How to contribute

1. **Open an issue first.** Say what you want to change and why. It gives the change somewhere to be argued about before it becomes a diff.

2. **Fork the repo and branch.** `feat/short-description` for a feature, `fix/short-description` for a bug.

3. **Change one file.** `desktop/plugin.js`, and nothing else. No build step, no npm dependency, no second module. Only `react`, `react/jsx-runtime` and `@hermes/plugin-sdk` resolve at runtime, and the host supplies all three.

4. **Test it in the app.** `node --check` catches syntax errors:

   ```bash
   node --check desktop/plugin.js
   ```

   That is everything Node can tell you. Loading the file with `node -e "import('./desktop/plugin.js')"` fails with `Cannot find package 'react'`, because React comes from the app and not from Node: that error is expected, and it is not a fault in your change. The dependable test is the app. Save the file, the plugin reloads in place, open the popover, and check it in both light and dark themes.

5. **Open the pull request.** Link the issue, and describe what changed and why. This repo ships a comment-free `plugin.js`, because the app imports it verbatim, so the PR description is where the reasoning is kept. Attach a screenshot if the change affects the UI.

## Rules the project does not bend

- **One file, no dependencies, no build step.** A disk-loaded plugin is imported as-is. There is no bundler to resolve anything else.
- **The reasoning goes in the PR, not the file.** The shipped file carries no comment, and a comment in it is stripped before release.
- **Nothing new leaves the machine.** The plugin sends the city name you type to the Open-Meteo city search to resolve coordinates, the resolved coordinates themselves, and, while auto-location is on, your public IP to ipwho.is. There is no credential anywhere in it, and that is a rule rather than a gap: do not add an endpoint that needs a key or sends anything else.
- **Hand-drawn SVG only.** Every chart and icon is inline SVG. Do not add a chart library.
- **The version has one home.** `const VERSION` near the top of the file. The release tag and the chip's hover marker both read it, so a release is that one line.
- **Commit messages** take the conventional prefixes: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`.

## In scope

- Open-Meteo fields the widget does not show yet: visibility, surface pressure, dew point
- UI refinements: layout, accessibility, responsive edge cases
- Bug fixes for edge-case locations, timezone drift, or an Open-Meteo API change

## Out of scope

- npm dependencies or a build pipeline
- Splitting the plugin into several files

## Code of Conduct

Read and follow the [Code of Conduct](CODE_OF_CONDUCT.md). Assume good faith.
