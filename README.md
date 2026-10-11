# APEX Rush

An offline coastal racing game built with JavaScript and Three.js, with a signed Android APK and a self-contained HTML edition.

Race along a six-kilometre highway, find safe gaps in traffic, manage nitro and finish before the timer expires. APEX Rush focuses on readable controls, smooth arcade handling and rewarding clean driving.

![APEX Rush cockpit view](docs/images/cockpit.png)

**Release:** 1.5.0 · **Automated checks:** 93 passed · **Platforms:** modern browsers and Android 8+

## Play and download

- [Download the Android APK](downloads/APEX-Rush.apk?raw=true).
- [Open or download the standalone HTML](index.html). On GitHub, use the file's download button; the code preview does not run the game.
- [Read the development report](docs/Development-Report.pdf).
- For a playable website, enable GitHub Pages using the instructions in [GITHUB_SETUP.md](GITHUB_SETUP.md). The repository already includes a root `index.html` and `.nojekyll`.

The HTML embeds its scripts, fonts and rendering library. The APK packages the same game and declares no Internet permission. The hosted Pages edition requires an initial network load; it is not an offline-installable PWA. Download the HTML or use the APK for the offline edition.

## Features

- Smooth acceleration, progressive braking, aerodynamic drag and speed-dependent steering response.
- Steering inertia and damped recovery, with road-edge assistance and bounded lateral movement.
- Chase and cockpit views with live speed, gear and route information.
- Procedural coastal terrain, ocean, palms, vegetation, buildings, guardrails and roadside lighting fixtures.
- Deterministic traffic with an open lane in each wave.
- Collision separation, short body compression and rebound, and restrained camera feedback.
- Speed, time and nitro penalties for collisions; rewards for clean passes and clean streaks.
- An interactive steering, braking and nitro tutorial.
- Three difficulty modes, bronze/silver/gold ratings and local best records.
- Keyboard and multi-touch controls, optional sound and reduced-motion support.
- High/Lite rendering options and a Canvas compatibility fallback.

## Controls

| Action | Keyboard | Touch |
| --- | --- | --- |
| Steer | Left/Right arrows or A/D | Left/Right buttons |
| Brake | Down arrow or S | Brake button |
| Nitro | Hold Space | Hold lightning button |
| Camera | C | View button |
| Pause | P or Escape | Pause button |
| Sound | Sound button | Sound button |

Acceleration is automatic. Android Back moves from race to pause, then to the menu, then exits the app.

## Race rules

| Mode | Time limit | Contact limit | Time lost per contact | Gold | Silver |
| --- | ---: | ---: | ---: | ---: | ---: |
| Cruise | 112 s | 8 | 1 s | 85 s | 100 s |
| Sport | 90 s | 4 | 3 s | 80 s | 86 s |
| Pro | 82 s | 3 | 4 s | 79 s | 80 s |

Gold requires a finish within the gold time and zero contacts. Silver requires a finish within the silver time and at most one contact. Other successful finishes receive bronze. Reaching the contact limit or running out of time ends the run.

## Run locally

Install Node.js 22 or newer, then run these commands in the repository folder:

```sh
npm start
```

Open **http://127.0.0.1:4173/**. No npm dependency installation is required: the runtime library and fonts are already bundled.

The preview serves the editable multi-file game from `apex-rush/dist/`. The root `index.html` is the generated, single-file distribution used by GitHub Pages. Edit the source files and rebuild to update it.

## Test and build the HTML

```sh
npm test
npm run build
```

The test command runs 93 deterministic simulation and controller checks. The build regenerates root `index.html` with embedded scripts, styles, fonts and Three.js modules. Commit that generated file when publishing changes to GitHub Pages.

## Build the Android APK

The included build script targets Windows PowerShell and requires:

- Android SDK platform 34 and build-tools 36.1.0.
- A JDK, supplied through `JAVA_HOME` or the Android Studio bundled JDK.
- `ANDROID_HOME` pointing to the SDK directory, or the default local Android SDK location.

```powershell
.\android\build.ps1
.\check-final-package.ps1
```

The generated package is `android/build/APEX-Rush.apk`. The verification script compares its assets with the current source. The build script creates a local demonstration signing key on first use. **Signing keys are excluded from this repository.** A locally generated key differs from the key used for the downloadable APK; Android will not accept it as an update over an installation signed with another key. Uninstalling an existing version also removes its local records.

The bundled signing configuration is for demonstration builds. It is not a production release-key management system or evidence of Play Store approval.

## Project structure

```text
index.html                  Generated standalone game and Pages entry point
apex-rush/dist/
  index.html                Editable interface
  engine.js                 Driving, traffic, collision and scoring rules
  world.js                  Coordinates, terrain and object placement
  renderer3d.js             Three.js scene, materials and camera
  game.js                   Input, screens, audio and Canvas fallback
  storage.js                Local record validation and persistence
  vendor/                   Bundled Three.js and its licence
  fonts/                    Bundled Barlow fonts and their licence
android/                    Native Java host, resources and build script
downloads/APEX-Rush.apk      Signed demonstration APK
docs/                       Report, screenshots and verification evidence
test-final.cjs              Simulation and controller tests
build-html.cjs              Standalone HTML bundler
preview.cjs                 Local preview server
```

## Technology and design

The game uses HTML, CSS and JavaScript, Three.js 0.180.0, WebGL, Canvas, Web Audio and localStorage. Android packaging uses a Java WebView Activity and Android SDK command-line tools. There is no server, login, cloud database, Unity project or external physics engine.

The simulation advances in steps of at most 1/120 second. Rendering is separate from the simulation, allowing the same rules to drive both 3D and compatibility graphics. Static scenery uses a common world translation; repeated roadside geometry uses instancing. The full [development report](docs/Development-Report.pdf) explains the implementation and cites official documentation.

## Verification and limitations

- **93/93 automated checks passed** for the final release; see [test evidence](docs/test-results.json).
- Browser review included desktop and phone-sized landscape layouts, tutorial, driving controls and collision feedback.
- APK signing and packaged asset checks passed; see [package evidence](docs/package-checks.json).
- **Physical Android testing has not been completed.** Installation, app switching, screen locking, cutouts and sustained performance still need testing on the target phone.
- The standalone HTML was reviewed through a local server. Direct file URL launch was not independently verified in the review browser; file-origin storage behaviour can vary.
- This is a procedural arcade racer, not a photorealistic simulator. It has no vehicle deformation, live mirrors, full suspension simulation or open world.
- This is a student application-development project, not a clinically validated healthcare or mental-health tool.

## Development and attribution

Development used iterative requirements, AI-assisted coding with Codex, source inspection, automated tests and browser review. The public TurboRush reference informed control visibility and roadside depth; its source code and assets were not copied.

Three.js and Barlow retain their respective MIT and SIL Open Font License notices. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). No project-wide open-source licence has been selected for the original game code; do not assume the bundled library licences cover the whole project.
