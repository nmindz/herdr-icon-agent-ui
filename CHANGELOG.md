# Changelog

All notable changes to this project will be documented in this file.

## [2.0.0](https://github.com/nmindz/herdr-icon-agent-ui/compare/v1.3.0...v2.0.0) (2026-09-08)

### ⚠ BREAKING CHANGES

* plugin id changes from qintmb.herdr-icon-agent-ui to
nmindz.herdr-icon-agent-ui. This moves the plugin config-dir and the
GitHub install source; existing installs under the qintmb id must
uninstall and reinstall from nmindz/herdr-icon-agent-ui.

### Features

* alias dsh/deepseek-harness agent labels to the deepseek logo ([492cb89](https://github.com/nmindz/herdr-icon-agent-ui/commit/492cb89bb5a7f1747a207f35841b5f1856796723))
* rebrand plugin id and install source to the nmindz fork ([51b4c95](https://github.com/nmindz/herdr-icon-agent-ui/commit/51b4c95a02dfd79c40b1b835d307b6cccf1487c7))

### Bug Fixes

* exclude test_font from the dependency-free CI test job ([2c2f838](https://github.com/nmindz/herdr-icon-agent-ui/commit/2c2f838a8e9f45b309305134d3130ff15fc271de))
* pin conventional-changelog-conventionalcommits to v8 in release workflow ([a8b12da](https://github.com/nmindz/herdr-icon-agent-ui/commit/a8b12daac8e99c5c3314ad7b39a445233e42865c))

### Documentation

* inline mp4 video, drop svg icon grid ([6cb839f](https://github.com/nmindz/herdr-icon-agent-ui/commit/6cb839f2766f72885e1d36286e93ca63c658e798))

## [1.3.0] - 2026-08-20

### Added
- **Dot-free line + LED state marks** (à la [lfsmoura/led-agent-status](https://github.com/lfsmoura/led-agent-status)). Each `$state_*` token now holds the whole line `⟨glyph⟩ ⟨logo⟩ ⟨name⟩`, so Herdr's `·` cell separator no longer appears between spinner, logo, and name. `blocked`/`idle`/`unknown` use a full-size LED `⬤`; `done` a `✓`; `working` the braille spinner.
- **"done" hold**: the animator detects `working → idle/done` and holds the green `✓` for `DONE_HOLD_SECONDS` (default 6 s) before falling to idle — Herdr otherwise collapses `done` into `idle` instantly, so a finished turn never showed done.

### Changed
- Line is coloured per status via the row `fg` (one colour per line), trading per-agent brand logo colour for a clean dot-free line. Recommended `rows_by_agent` updated accordingly.

## [1.2.1] - 2026-08-20

### Fixed
- State glyphs showed raw ANSI text (`[38;2;217;…m`) in the sidebar — Herdr renders token values as plain text. Replaced the single ANSI-coloured `$agent_state` token with **one bare-glyph token per state** (`$state_working`, `$state_done`, `$state_blocked`, `$state_idle`, `$state_unknown`); colour now comes from each cell's `fg` in `rows_by_agent`. Only one token is set per pane, the rest cleared.

## [1.2.0] - 2026-08-20

### Added
- `agent_state.py` — animated lifecycle-state glyph via the `$agent_state` token: orange braille spinner while `working`, green `✓` on `done`, red `■` on `blocked`, dim `◦`/`?` for `idle`/`unknown`. Coloured with inline ANSI, no emoji.
- Background animator (150 ms frame cadence) spawned on demand and self-terminating ~1.8 s after no agent is working, so it costs nothing on an idle machine. Single-writer PID lock; `state-stop` action / `--stop` to halt.
- Manifest: `pane.agent_status_changed` event, extra `[[startup]]`/`[[events]]` pass, and `state-start`/`state-stop` actions.
- `tests/test_agent_state.py` — glyph mapping, frame cycling, no-emoji assertions.
- README: recommended `rows_by_agent` layout with `$agent_state` + `$harness_logo`, and a lifecycle-state reference table.

## [1.1.0] - 2026-08-18

### Added
- 5 new agent icons: `copilot` (`U+E1AC`), `deepseek` (`U+E1AD`), `gemini` (`U+E1AE`), `gpt` (`U+E1AF`), `qwen` (`U+E1B0`) — 17 glyphs total. Marks from [lobehub/lobe-icons](https://github.com/lobehub/lobe-icons) (MIT).
- `tools/normalize_svg.py` — strips `<title>`, `<desc>`, fills, and wrapper elements from a downloaded mark.
- `build_font.py` now rejects SVGs containing anything other than `<svg>`/`<path>` instead of silently dropping outlines, and self-checks every built glyph against its source aspect ratio.
- `tests/test_font.py`: `test_aspect_ratio_matches_source_svg`.

### Fixed
- Glyphs no longer distorted: replaced the non-uniform `580×1040` stretch with a uniform scale into a `560×760` cell, so each mark keeps its source aspect ratio. Wide marks (`mastracode`, `claude`) were flattened before.
- Vertical centring moved to `CENTER_Y = 365` (midpoint of JetBrains Mono cap height 730) so icons sit on the same optical line as adjacent text.

### Changed
- All source SVGs re-normalized to bare `<svg viewBox>` + `<path d>`. `claude.svg` replaced with the full Anthropic starburst (the previous file was a 24×15 crop that read as a flat bar).
- Ghostty `font-codepoint-map` range widened to `U+E1A0-U+E1B0`.

## [1.0.0] - 2025-08-18

### Added
- Initial release: 12 agent icons (claude, codex, opencode, omp, cline, mastracode, kimi, kilo, maki, pi, hermes, cursor)
- Font `Herdr Agent Icons Max` with metrics matching JetBrains Mono (UPM 1000, advance 600, ascent 1020, descent -300)
- Non-uniform glyph scale (580×1040) so icons align with terminal text height
- Variant system: auto, font, text, none
- Runtime plugin `agent_icons.py` using stdlib only (Python ≥ 3.11)
- Build script `tools/build_font.py` with deterministic output
- Preview tool `tools/preview.py` for selecting variant
- Sidebar color examples in README
- Comprehensive documentation
