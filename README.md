# Pikachu Glyph Toy

A custom [Glyph Toy](https://github.com/Nothing-Developer-Programme/GlyphMatrix-Developer-Kit)
for the **Nothing Phone (3)**. Displays Pikachu's face on the 25×25 LED matrix on the back
of the phone, and plays a "PikaaaaaaaaaCHUUUU!" scream animation — mouth opens wide, lightning
bolts blast out from the cheeks — when you long-press the Glyph Button. Optional: drop in a
`pikachu_scream.mp3` and the phone will scream audibly too.

---

## Contents

1. [What you need](#1-what-you-need)
2. [Clone and open in Android Studio](#2-clone-and-open-in-android-studio)
3. [Drop in the Nothing SDK AAR](#3-drop-in-the-nothing-sdk-aar)
4. [Build and install to your phone](#4-build-and-install-to-your-phone)
5. [Activate the toy on your phone](#5-activate-the-toy-on-your-phone)
6. [Publish to GitHub](#6-publish-to-github)
7. [Submit to Nothing Playground](#7-submit-to-nothing-playground)
8. [Add a scream sound (optional)](#8-add-a-scream-sound-optional)
9. [Customize the design](#9-customize-the-design)
10. [How the code works](#10-how-the-code-works)
11. [Troubleshooting](#11-troubleshooting)
12. [Legal / IP](#12-legal--ip)

---

## 1. What you need

| What | Why |
|---|---|
| **Nothing Phone (3)** | The toy targets `DEVICE_23112`. Phone (4a) Pro has a 13×13 matrix and won't work without changes. |
| **Android Studio** (Otter or newer — anything from 2024.x onward is fine) | To compile and push the APK to the phone. [Download here](https://developer.android.com/studio). |
| **A USB-C cable** | For installing the APK to the phone over ADB. |
| **GitHub account** | To publish the repo and submit to Playground. |
| **~20 minutes** | Most of it waiting for Gradle to download things on first build. |

You do **not** need to be a developer. If you can click "Run" in Android Studio, you can do this.

---

## 2. Clone and open in Android Studio

```bash
git clone https://github.com/YOUR-USERNAME/PikachuGlyph.git
cd PikachuGlyph
```

Then in Android Studio: **File → Open…** → pick the `PikachuGlyph` folder. Wait for Gradle
sync to finish (status bar at the bottom — first sync can take a couple of minutes while it
downloads the Android SDK, Gradle distribution, and dependencies).

If Gradle complains about an SDK path, go to **File → Project Structure → SDK Location**
and make sure it points at your installed Android SDK.

---

## 3. Drop in the Nothing SDK AAR

The Glyph Matrix SDK is a binary (`.aar`) that Nothing publishes separately. It's not in
this repo — you have to fetch it once:

1. Go to [**GlyphMatrix-Developer-Kit**](https://github.com/Nothing-Developer-Programme/GlyphMatrix-Developer-Kit).
2. Click **`glyph-matrix-sdk-2.0.aar`** in the file list.
3. Click the **download raw file** button (small arrow icon on the right).
4. Move the downloaded `.aar` into this project's `app/libs/` folder.

After this step, `app/libs/` should contain exactly one file like:

```
app/libs/glyph-matrix-sdk-2.0.aar
```

Back in Android Studio: **File → Sync Project with Gradle Files**. The red squiggles on
`com.nothing.ketchum.*` imports should disappear.

---

## 4. Build and install to your phone

1. On the **Phone (3)**: enable **Developer Options** and **USB debugging**.
   - Settings → About phone → tap **Build number** 7 times to unlock Developer Options.
   - Settings → System → Developer options → enable **USB debugging**.
2. Plug the phone into your computer with a USB-C cable. The phone will pop a dialog
   asking you to allow debugging from this computer — accept it.
3. In Android Studio, the phone should appear in the device dropdown at the top. Pick it.
4. Press the green ▶ **Run 'app'** button.

Gradle will build the APK, push it to the phone, and launch the app. The first build
takes a minute or two; incremental builds after that are fast.

You should see a yellow **Pikachu Glyph** app on your phone with a single
**Open Glyph Toys Manager** button.

---

## 5. Activate the toy on your phone

Installing the APK doesn't automatically enable the toy in the Glyph carousel — you have
to add it to the active toys list:

1. Tap **Open Glyph Toys Manager** in the app (or go to Settings → Glyph Interface → Glyph
   Toys manually).
2. You'll see **Pikachu Scream** in the "All toys" / inactive list.
3. Tap the **+** button next to it to move it into the active list.
4. Flip the phone over and **short-press the Glyph Button** on the back. Cycle through
   your active toys until Pikachu's face shows up.
5. **Long-press the Glyph Button**. Watch Pikachu scream. 🌩️

Short press = next toy. Long press (while Pikachu is showing) = fire the scream animation.

---

## 6. Publish to GitHub

From inside the project folder:

```bash
git init
git add .
git commit -m "Initial commit: Pikachu Glyph Toy"
```

Then create a new repo on [github.com/new](https://github.com/new) (make it **public** if
you want to submit to Playground), and:

```bash
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
git branch -M main
git push -u origin main
```

**Important:** the `.gitignore` in this project specifically allows `.aar` files under
`app/libs/` so the SDK travels with the repo. If you're worried about redistributing
Nothing's binary, edit `.gitignore` to exclude it and tell contributors to download it
themselves (the `app/libs/README.md` already has instructions).

---

## 7. Submit to Nothing Playground

[Playground](https://playground.nothing.tech/) is Nothing's community showcase. Once your
repo is public on GitHub:

1. Go to [playground.nothing.tech](https://playground.nothing.tech/) and sign in with your
   Nothing account.
2. Find the **Submit** / **Add your Glyph Toy** flow (the exact button name may change —
   as of this writing it's in the top-right navigation).
3. Paste your **public GitHub repo URL**.
4. Fill in the toy name (`Pikachu Scream`), a short description, and upload a screenshot
   or short video of the animation running on the matrix.
5. Submit for review.

Nothing's moderators vet submissions. If your submission is rejected for IP reasons (see
[§11](#11-legal--ip)), you can either make the repo private and keep the toy for yourself,
or redesign the art to avoid protected characters.

---

## 8. Add a scream sound (optional)

The phone has a speaker, so the toy *can* scream out loud — separate from the LED
animation.

### What's bundled

This repo ships with `app/src/main/res/raw/pikachu_scream.mp3` — a ~4.7 second clip
already wired up. The LED animation timing (`FRAME_DELAYS_MS` in
`PikachuToyService.kt`) is tuned to roughly match that length. Build and install, and
the phone will scream when you long-press the Glyph Button. No extra setup.

### Swapping it out

To use a different clip, just drop your own file into `app/src/main/res/raw/`,
named `pikachu_scream.mp3` (or `.ogg` / `.wav` / `.m4a` — the base filename is what
matters), replacing the existing one. If your clip is a different length, scale the
numbers in `FRAME_DELAYS_MS` proportionally.

### If you remove the audio file entirely

The code looks the file up by name at runtime, so if you delete
`pikachu_scream.mp3` the project still builds and runs — the LEDs animate silently.

### About the bundled clip

The included audio appears to be sourced from Pokémon media. This is a non-commercial
fan-project demo; publishing this clip on a public repo or submitting it to
Playground carries IP risk (see [§12](#12-legal--ip)). If that concerns you, swap the
file for one you recorded yourself or a CC-licensed alternative before pushing to
GitHub, or add `app/src/main/res/raw/pikachu_scream.*` to `.gitignore`.

### How loud is it?

The audio plays through the phone's main media stream, so the hardware volume
buttons control it. No ducking, no audio focus — if you have Spotify playing,
Pikachu will shout over it.

---

## 9. Customize the design

All the art lives in **one file**: `app/src/main/java/com/pikachu/glyph/PikachuArt.kt`.

Everything is procedural (built from circles, triangles, and zigzags), so small edits
give you big visual changes:

| Want to change… | Edit this in `PikachuArt.kt` |
|---|---|
| Face size | `HEAD_R` (default 9). Bigger = face fills more of the matrix. |
| Face position | `HEAD_CY` (default 16). Lower = more room for ears above. |
| Cheek size or position | `LEFT_CHEEK_X`, `RIGHT_CHEEK_X`, `CHEEK_Y`, `CHEEK_R` |
| Eye spacing | `LEFT_EYE_X`, `RIGHT_EYE_X`, `EYE_Y` |
| Mouth position | `MOUTH_CX`, `MOUTH_CY` |
| Ear shape | The `Triple(col, yMin, yMax)` lists in `drawEars()`. Each triple is one column of the ear. |
| How bright each part is | `FACE_FILL`, `FACE_EDGE`, `CHEEK_GLOW`, `DIM_EAR_TIP` (0–255) |
| How open the mouth gets mid-scream | `openness` range in `drawMouth()` |
| Lightning bolt shape | `drawZigzag()` parameters in `drawLightning()` |

Animation timing lives in `PikachuToyService.kt` in the `FRAME_DELAYS_MS` array — each
number is how many milliseconds to hold that frame. Stretch the "Pikaaaa" hold by raising
indices 3–5, or make the "CHU" snappier by lowering indices 6–11.

### Replacing Pikachu with a different design

The service only depends on `PikachuArt.idle()` and `PikachuArt.screamFrame(step)`.
Rewrite those two functions to return any 625-element `IntArray` of brightness values
(0–255) and you have a totally different toy. You can even ignore the helper primitives
and hand-code pixels — `frame[y * 25 + x] = brightness` is the whole API.

---

## 10. How the code works

Three small files do everything:

- **`PikachuArt.kt`** — Pure Kotlin, no Android dependencies. Takes a frame index and
  returns a 625-int array where each int is an LED brightness (0 = off, 255 = max). Uses
  drawing primitives (`fillCircle`, `punchCircle`, `drawZigzag`) to build Pikachu up from
  shapes.

- **`PikachuToyService.kt`** — An Android `Service` that Nothing's Glyph framework binds to
  when the user picks this toy from the carousel. On bind, it connects to
  `GlyphMatrixManager`, registers as `Glyph.DEVICE_23112` (Phone 3), and shows the idle
  frame. It listens for `GlyphToy.EVENT_CHANGE` (the long-press event) and fires the
  scream animation as a chain of `Handler.postDelayed` callbacks — no coroutines, no
  threads, just one frame at a time on the main looper. If a `res/raw/pikachu_scream.*`
  file exists, it also kicks off a `MediaPlayer` in parallel when the scream starts.

- **`MainActivity.kt`** — A simple welcome screen with one button that launches the system
  Glyph Toys Manager. That's it.

The `AndroidManifest.xml` wires the service to the framework via the
`com.nothing.glyph.TOY` intent filter and the `longpress=1` metadata flag.

Rough flow:

```
user long-presses Glyph Button
       │
       ▼
 Nothing system sends MSG_GLYPH_TOY / EVENT_CHANGE to our service
       │
       ▼
 PikachuToyService.playScream()
       │
       ├─► postDelayed(80ms)  → setMatrixFrame(screamFrame(1))  "Pi"
       ├─► postDelayed(80ms)  → setMatrixFrame(screamFrame(2))  "ka"
       ├─► postDelayed(250ms) → setMatrixFrame(screamFrame(3))  "aaa..."
       ├─► ...                                                    CHUUUU!
       └─► postDelayed(120ms) → setMatrixFrame(idle())           back to calm
```

---

## 11. Troubleshooting

**"Cannot resolve symbol GlyphMatrixManager" in Android Studio.**
The AAR isn't in `app/libs/` yet, or Gradle hasn't re-synced since you added it. Drop the
AAR in and then do **File → Sync Project with Gradle Files**.

**Build succeeds, but the toy doesn't appear in Glyph Toys Manager.**
Check your phone's system version — `com.nothing.glyph.TOY` support requires the firmware
that shipped with Phone (3). Settings → About phone → Software info → Nothing OS version
should be 3.5 or newer. If it's older, update via **Settings → System → System update**.

**Toy appears but screen is black when selected.**
The app failed to register with the Glyph service. Most common cause: the permission
`com.nothing.ketchum.permission.ENABLE` was stripped by another manifest tool. Open
`app/src/main/AndroidManifest.xml` and confirm the `<uses-permission>` line is still there.
Also check `adb logcat | grep PikachuToyService` for stack traces.

**Long-press does nothing — no animation fires.**
Either (a) the `longpress=1` metadata is missing from the manifest service block, or (b)
the toy isn't currently *the one showing* on the matrix. Long-press only targets the
active toy. Short-press until Pikachu shows, then long-press.

**Animation plays once, then stops working.**
`isAnimating` got stuck because the service crashed mid-animation. Kill the app
(force-stop it in settings) and re-cycle to Pikachu. If this repeats, share the logcat
output — there's probably an exception being thrown in `setMatrixFrame`.

**Pikachu looks weird / features are in the wrong place.**
That's purely a `PikachuArt.kt` issue — nothing in the rest of the code affects visuals.
Adjust the `HEAD_CX` / `HEAD_CY` / etc. constants and rebuild.

**No sound when Pikachu screams, but the LED animation works.**
Check that `app/src/main/res/raw/pikachu_scream.mp3` (or `.ogg`/`.wav`/`.m4a`) actually
exists with that exact base filename. If it does, run `adb logcat | grep PikachuToyService`
— if you see `No audio file at res/raw/pikachu_scream.*`, the filename doesn't match. If
you see a `MediaPlayer error`, the file is corrupt or in an unsupported format.

**Sound plays but video/LED is out of sync.**
Trim your audio file to ~1.8 seconds to match the LED animation duration, or adjust
`FRAME_DELAYS_MS` in `PikachuToyService.kt` to stretch the animation to match your clip.

---

## 12. Legal / IP

Pikachu and all related Pokémon marks are trademarks of **Nintendo, Game Freak, and The
Pokémon Company**. This project is a non-commercial fan work. Depicting trademarked
characters in public distributions (GitHub, Playground) may result in takedown requests.

Your options:

- **Keep the repo private** and use the toy only on your own phone — no issue.
- **Keep the repo public** — accept that Nothing or the rights holders may ask for it to
  be taken down. No license grant in this repo gives you permission to use Pokémon IP;
  that's up to Nintendo/TPCi.
- **Reskin it** — change the art in `PikachuArt.kt` to a generic "electric mouse" with
  similar vibes but no protected features, and rename the toy to something original.

If you're a rights holder and want this repo taken down, open an issue or contact the
repo owner directly.

---

## Credits

- The Glyph Matrix SDK and documentation belong to **Nothing Technology Ltd**.
- Pixel art and animation logic: this repo.
- Pikachu character design: **Nintendo / Game Freak / The Pokémon Company**. Used here
  as non-commercial fan art, not licensed.

---

*Built for the Phone (3). Have fun. Don't be a jerk.*
