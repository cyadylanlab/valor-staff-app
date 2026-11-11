# Valor Staff

A native app wrapper around the Virginia Valor FC staff tools. The home screen is
bundled in the app. The tools themselves load from their live URLs, so when you
deploy a change to a tool the app picks it up with no rebuild and no app store
resubmission.

App ID: `org.virginiavalorfc.staff`

---

## What is in here

| Path | What it is |
| --- | --- |
| `www/index.html` | The home screen. The tool list lives at the bottom of this file. |
| `www/crest.png` | Club crest, used on the home screen. |
| `assets/` | Source images the app icon and splash screens are generated from. |
| `capacitor.config.json` | App name, app ID, and the list of domains the app may open. |
| `.github/workflows/android.yml` | Builds the Android APK on GitHub. |

The `android/` and `ios/` folders are not committed. They are generated from the
files above, by GitHub Actions for Android and by you on a Mac for iPhone.

---

## Changing the tool list

Open `www/index.html` and scroll to the bottom. The list looks like this:

```js
var TOOLS = [
  { name:"Tournament Selections",
    blurb:"Submit and review tournament choices for your teams.",
    url:"https://cyadylanlab.github.io/tournament-list/" },
  ...
];
```

Add, remove or re-point an entry, commit the change, and the next build picks it up.
Order in this list is the order on the wall.

If you add a tool on a domain that is not already covered, add that domain to
`allowNavigation` in `capacitor.config.json` as well, or the app will refuse to open it.

---

## Getting the Android APK

1. Push this repo to GitHub as `cyadylanlab/valor-staff-app`.
2. Go to the **Actions** tab. The build starts on its own after a push, or you can
   start one with **Run workflow**.
3. When it finishes, open the run and download **valor-staff-app-apk** from the
   Artifacts section at the bottom.
4. Unzip it and send `app-debug.apk` to any Android phone. The phone will ask for
   permission to install from an unknown source, which is expected for a test build.

This is a debug build, which is right for testing. Publishing to Google Play needs a
signed release build and a one-time $25 developer account.

---

## Getting the iPhone app

This part needs a Mac. It cannot be done from a phone or a PC.

1. Install Node.js and Xcode on the Mac.
2. Clone this repo, then in the project folder run:

   ```
   npm install
   npx cap add ios
   npx capacitor-assets generate --ios
   npx cap sync ios
   npx cap open ios
   ```

3. Xcode opens. Select the **App** target, then the **Signing & Capabilities** tab,
   and pick your team under Signing. This is where the Apple Developer Program
   account is required, at $99 per year.
4. To put it on your own phone for testing, plug the phone in, pick it as the run
   destination, and press Run.
5. To send it to staff, choose **Product** then **Archive**, upload to App Store
   Connect, and distribute through TestFlight.

TestFlight is the sensible route for a staff app. It holds up to 10,000 testers,
installs like a normal app, and skips full App Store review.

---

## A note on the tool URLs

The home screen points at GitHub Pages URLs, for example
`https://cyadylanlab.github.io/valorwebsite/best-xi.html`. These load the tool on its
own, without the Wix page around it, which is what you want inside an app.

Confirm each one opens in a phone browser before you hand the app out. If GitHub Pages
is not switched on for a repo, either enable it in that repo's Settings under Pages, or
change that entry in the tool list to the Wix address instead.
