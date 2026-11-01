# Changelog

## 2026-11-01: Motion layer

1. The Call, Website and Directions buttons on every resource card lift 5px on hover and keyboard focus. Their icons sit before the label, so the icons stay put instead of nudging right.
2. No scroll reveals: the landing page is a single screen (title, map, footer strip), and the panels are built in the browser after load. Crisis lines and resource cards appear at once, without waiting on an animation.
3. whysice-motion.css and whysice-motion.js, shared with the other Whysice sites, are linked after the page stylesheet with the script deferred, and the service worker precaches both (cache v2). With reduced motion on, nothing moves.
