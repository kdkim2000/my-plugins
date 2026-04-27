## Summary

Adds dark mode support to the app, giving users control over the interface theme. The selected preference is persisted to `localStorage` so users don't have to re-apply their choice on every visit, and a toggle button in the header makes switching themes quick and discoverable.

## Changes

- **Header**: Added a dark/light mode toggle button that is always accessible from the top navigation
- **Theme logic**: Implemented dark mode styles and toggling mechanism applied at the app level
- **Persistence**: User's theme preference is saved to and loaded from `localStorage` so the chosen mode survives page refreshes and new sessions

## How to Test

1. Open the app in a browser
2. Click the toggle button in the header to switch to dark mode
3. Verify that the UI switches to a dark color scheme
4. Refresh the page and confirm dark mode is still active (preference was persisted)
5. Click the toggle again to switch back to light mode
6. Refresh and confirm light mode is restored
7. Open DevTools → Application → Local Storage and verify a theme key is present with the correct value

## Notes for Reviewers

The preference is stored entirely client-side via `localStorage`. If a user clears their browser storage, the app will fall back to the default (light) mode. A follow-up could respect the OS-level `prefers-color-scheme` media query as an initial default before any explicit user choice is made — out of scope for this PR but worth considering.
