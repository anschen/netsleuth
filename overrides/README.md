DevTools overrides
==================

This folder contains patched versions of DevTools code necessary to get everything working.  The files in /deps/devtools-frontend are an entirely unmodified checkout from the [DevTools repo](https://github.com/ChromeDevTools/devtools-frontend).

When the browser requests a DevTools file, the daemon's server looks here first.  If there is no file, then it serves the file from /deps/devtools-frontend.

Upgrading DevTools
------------------
Because we have to patch some DevTools files, upgrading the DevTools release is a relatively painful process.  Generally:

1. Generate a diff of /deps/devtools-frontend and /overrides, and keep it handy.
2. Get the latest release from the DevTools repo.
3. Replace /deps/devtools-frontend with the /front_end folder from the DevTools repo.
4. InspectorBackendCommands.js and SupportedCSSProperties.js are files generated by a DevTools build process.  It is a giant pain to get the necessary build scripts working, so it might be easier to simply grab [recent versions from a release](https://unpkg.com/browse/chrome-devtools-frontend/front_end/).
5. Copy the new SupportedCSSProperties.js and InspectorBackendCommands.js files to the /overrides folder.  Add the `Gateway.*` event registration calls (as seen in the diff).
6. Manually re-apply the other patches seen in the step 1 diff as necessary.
7. It may be necessary to update the default DevTools settings that we save to `localStorage` in /www/ns.js.
8. Test the UI throughly and hope that upstream changes don't break us too badly.