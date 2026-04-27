## Add Dark Mode Support

### Summary

This PR introduces dark mode support across the app, with user preference persistence and a convenient toggle in the header.

### Changes

- **Dark mode implementation**: Applied dark mode styles throughout the app using CSS variables / theme classes, ensuring all UI components respond correctly to the selected theme.
- **localStorage persistence**: The user's theme preference is saved to `localStorage` so it is automatically restored on subsequent visits — no need to re-select the preference each session.
- **Header toggle button**: Added a dark/light mode toggle button in the app header, giving users quick and easy access to switch themes from any page.

### How It Works

1. On app load, the saved preference is read from `localStorage`.
2. If a preference exists, it is applied immediately to avoid a flash of unstyled content.
3. When the user clicks the toggle button in the header, the theme switches and the new preference is written back to `localStorage`.

### Testing

- [ ] Toggle switches between light and dark mode visually.
- [ ] Preference persists after a full page reload.
- [ ] Preference persists after closing and reopening the browser tab.
- [ ] All pages and components render correctly in both modes.
- [ ] No flash of incorrect theme on initial page load.
