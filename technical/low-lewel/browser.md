Why No GET Request Happens for SPA routing
- Interception: The SPA router intercepts navigation events (via click handlers or the popstate event).
- Client-Side Processing: URL changes are handled by JavaScript, which dynamically updates the page (using history API or hash routing).
- No Full Reload: The server isn’t contacted because the browser doesn’t perform its default behavior of reloading the page for every URL change.