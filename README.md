# CleanFrame TV (Google TV / Android TV)

A clean, glassy, Apple-inspired home screen for TCL Google TV / Android TV.
Inspired by https://github.com/nthnclrk/CleanFrame (Samsung/Tizen only), rebuilt natively for Android TV.

Status: source only. It has NOT been compiled or tested on a TV yet.

## Design and speed choices
- Native Jetpack Compose, no WebView (WebViews are slow on TV chips).
- "Glass" is semi-transparent gradients plus a thin highlight border. There is no live blur, because it is expensive on budget TV hardware.
- Focus animation uses only scale and alpha (GPU transforms).
- Icons are decoded once at 192px. The clock wakes once per minute.
- Pinned apps (Netflix, YouTube, Prime, Disney+, Apple TV, Canal+, ...) come first, then A-Z.

## Build without a computer (GitHub Actions, from an iPhone)
1. Create a free account at github.com, then create a new PUBLIC repository (e.g. "CleanFrameTV").
2. Upload the project files to it, keeping the folder structure. The `.github/workflows/build.yml` file must be included.
   Easiest: unzip in the iPhone Files app, then in the GitHub web page choose "Add file > Upload files".
   (Hidden folders like `.github` can be tricky on iOS. If it is missing, use "Add file > Create new file", type
   `.github/workflows/build.yml` as the name, and paste the workflow contents.)
3. Open the repo's Actions tab. The "Build APK" run starts by itself (or press "Run workflow").
4. When it is green (about 5 minutes), the APK is at:
   `https://github.com/<your-username>/<repo>/releases/download/latest/CleanFrameTV.apk`
5. On the TCL: install "Downloader" from the Play Store, enable "Install unknown apps" for it when asked,
   type that link, download, and install.
If the run fails, open it, copy the red error text and send it to Claude to fix.

## Build on a computer
1. Install Android Studio (current stable).
2. File > Open > the CleanFrameTV folder. Let Gradle sync (it creates the Gradle wrapper).
3. Build > Build APK(s). Output: app/build/outputs/apk/.

## Install on the TCL
1. TV: Settings > System > About > tap "Android TV OS build" 7 times to enable Developer options.
2. TV: Settings > System > Developer options > turn on USB debugging / Network debugging, and note the TV's IP address.
3. On your computer (with Android platform-tools): `adb connect <TV_IP>:5555` and accept the prompt on the TV.
4. `adb install -r app-debug.apk`
5. Open "CleanFrame" from your apps list to try it.

## Making it the Home screen (optional, read first)
Google TV usually does not let you pick a different default launcher. The common workaround is to disable the stock launcher.
1. Find the stock launcher package: `adb shell pm list packages | grep -i launcher`
   Typically `com.google.android.apps.tv.launcherx` (Google TV) or `com.google.android.tvlauncher` (Android TV). TCL may differ.
2. Disable it for your user: `adb shell pm disable-user --user 0 <package>`
3. Press Home. The TV should offer CleanFrame.

Recovery (keep this handy): `adb shell pm enable <package>` restores the stock launcher.
If the TV is stuck, connect with adb and run that command. Do not do step 2 until CleanFrame works when opened manually.

## Next ideas
Wallpaper picker, reorder/hide apps, HDMI input shortcuts, Art-style photo backgrounds.
