# Dark Star Origin — Directory

A searchable doctor / practice / location directory. Captures from post-it notes and
spreadsheet pastes. Runs entirely in the browser. No account, no server, no tracking.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole app |
| `manifest.webmanifest` | Lets it install to a home screen |
| `sw.js` | Offline cache, so it opens without signal |
| `icon-*.png` | App icons |

Keep all files in the same folder. `index.html` alone still works, it just won't install or run offline.

## Where the data lives

In the browser's `localStorage`, on the device that entered it. It never leaves that device.

Consequences worth knowing before you hand this to anyone:

- Each person has their own separate directory. Nothing syncs between them.
- Clearing browsing data for the site erases it. **Export a CSV now and then.**
- Safari on iOS deletes localStorage for sites you haven't opened in about 7 days —
  unless the app is installed to the home screen. Tell iPhone users to install it.
- Same origin = same data. A file opened from Downloads and the same app on a website
  are two different stores.

## Publishing it so coworkers can open a link

Any static host works. It's plain files, nothing to build or configure.

**Fastest (no account):** go to app.netlify.com/drop and drag the folder in. You get a
URL in about ten seconds. Send the URL around.

**Free and permanent:** GitHub Pages, Cloudflare Pages, or Netlify with an account.
Push the folder to a repo, turn on Pages, done.

**Inside the building:** drop the folder on any internal web server or shared drive that
serves over HTTP. It must be `https://` or `localhost` for offline install to work —
service workers are blocked on plain `http://` and on `file://`.

## Installing it as an app

- **iPhone / iPad:** open the link in Safari, Share button, *Add to Home Screen*.
- **Android:** open in Chrome, menu, *Install app* or *Add to Home Screen*.
- **Desktop:** Chrome or Edge shows an install icon in the address bar.

## Giving everyone the same starting directory

There's no shared database, so seed them instead:

1. Build the directory on your device.
2. **Import → Export directory as CSV.**
3. Send the CSV to your coworkers.
4. They open the app, go to **Import**, paste the CSV in, check the column matching, import.

From that point each copy drifts independently. Re-send a CSV whenever you want to resync.

## Publishing an update

Edit `index.html`, then bump the version in `sw.js`:

```js
const CACHE = "darkstar-v2";   // was v1
```

Without that bump, installed copies keep serving the old cached version.
