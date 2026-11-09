# Plan Comment: Block Playlists Feature

## Summary

I'm working on #4348 to add playlist blocking support. The blocklist currently handles only videos and channels because the detection logic requires a video ID and exits early if none is found. Playlists don't have video IDs (they use hrefs like `/podcasts` or `/watch?v=...&list=RD...`), so they're never processed.

My fix:
1. Add `blocklist.playlists` storage object
2. Extract playlist identifiers before the video ID check
3. Allow processing nodes with playlists but no video IDs
4. Add playlist blocking logic, UI menu section, and CSS class
5. Update all supporting functions (mutation observer, cleanup, button handler)

**Files:** 3 files, ~200 lines of changes
- `js&css/web-accessible/www.youtube.com/blocklist.js` (core logic)
- `menu/skeleton-parts/blocklist.js` (UI)
- `js&css/extension/www.youtube.com/styles.css` (styling)

## Implementation Details

**Storage:** Extends existing `blocklist` object with `playlists: {playlistId: {title: "..."}, ...}`

**Detection:** Extract playlist ID from node.href before video ID check; allow early return only if both video AND playlist IDs are missing

**UI:** Add third list button "Playlists" to settings menu (mirrors channels/videos pattern)

**Blocking:** Apply `it-blocklisted-playlist` CSS class to playlist tiles; remove class when unblocked

**Testing:** Jest test file with cases for storage init, ID extraction, CSS application, add/remove/persist

## Open Questions

1. **Playlist ID reliability:** Mix tiles may carry both video and playlist IDs in URLs. Need to verify whether blocking by playlist ID alone (vs. video ID alone) is the correct approach for all playlist types.

2. **Blocking scope:** Your issue asks "where to block playlists?" Options include home feed only, or also search results/sidebar/everywhere. Current plan assumes same scope as video blocking. Should clarify with you or infer from existing behavior.

Both questions can be resolved during implementation/testing.

## AI Disclosure

This plan was developed with AI assistance (Claude Haiku 4.5) during the CodePath AI301 course. The code analysis, testing strategy, and implementation steps are AI-generated but have been reviewed and will be verified during implementation.