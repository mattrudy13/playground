# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is `playground`, a small personal repo of standalone static HTML pages. There is no build system, package manager, or test suite — every page is plain HTML/CSS/vanilla JS that runs directly in a browser (open the file, or serve the directory with any static file server).

## Files

- `cfb-h2h.html` — College football head-to-head lookup. Fetches from the CollegeFootballData.com API (`https://api.collegefootballdata.com`), which has a dedicated `/teams/matchup` endpoint returning win/loss/tie totals directly.
- `cbb-h2h.html` — College basketball head-to-head lookup, same UI pattern. The CollegeBasketballData.com API (`https://api.collegebasketballdata.com`) has no matchup endpoint, so this page pulls all games for team1 via `/games?team=` and filters client-side for games against team2.
- `cfb-config.js` — Local-only config holding `window.CFBD_API_KEY`, loaded by both `*-h2h.html` pages via a `<script src="cfb-config.js">` tag (both APIs share one key). This file is gitignored and must never be committed; get a free key at https://collegefootballdata.com/key.
- `hello.py`, `kyle.md`, `swing.jpg` — unrelated scratch files, not part of any app.

## Architecture notes

The two `*-h2h.html` pages are structurally identical (same CSS, same picker/summary/table UI, same `apiKey()`/`apiFetch()`/`setStatus()` pattern) and cross-link to each other. When changing one, check whether the equivalent change applies to the other — they intentionally duplicate logic rather than sharing a JS file, since each is meant to be a single self-contained HTML file.

Both pages fail gracefully with no API key set (inputs still work, team autocomplete just won't populate) via the `apiKey()` check.
