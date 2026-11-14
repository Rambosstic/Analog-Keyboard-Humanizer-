# Analog Keyboard Humanizer

Turn an analog keyboard's left-stick output into more natural, adjustable controller movement. The adapter keeps your actual key depth and changes only the left stick. Buttons, triggers, and the right stick pass through unchanged.

> **Downloads are not published yet.** This repository is the public guide and download home. Board packages will appear under [Releases](https://github.com/Rambosstic/Analog-Keyboard-Humanizer-/releases), with a separate ZIP for each supported board. Choose your board's package, not GitHub's **Code → Download ZIP** or automatic **Source code** archives; those contain this repository's documentation, not installable firmware.

## Humanizer Studio

The included **Analog Gamepad Helper.html** now opens **Humanizer Studio**: a local tuner with separate Response, Return, Circularity, Angular Offset, Arc, Wander, and Thumb Emulation pages. **All Tunings** puts those controls on one page; **Profiles** and **Diagnostics** have their own sections.

- Compare incoming and tuned movement using **Processed only**, **Overlay**, or **Side by side** Live Preview.
- Read stick X/Y, magnitude, and angle; expand the preview or use a resizable **Pop out** window.
- Customize each trace's color, line/dash/dot style, thickness, opacity, and trail fade.
- Shape smoothing with Global, three-point Simple deflection, or an editable Advanced deflection curve.
- Save and share JSON config files, or keep named presets in your browser.
- Use **Press & release timing** and paired movement recordings to inspect what your settings do.

**Thumb Emulation is the sole optional Thumb helper.** Thumb Motion and Inner Gate have been removed. Simple mode offers one Strength slider; Advanced keeps six additional controls for those who want them.

## What you need

- An analog keyboard that can output a real **left stick in Gamepad/XInput mode** and save that mapping for use without its desktop software. The gamepad mode must output as Xinput/Xbox Controller. Direct input or other output modes are not supported.
- One supported adapter: [**Adafruit Feather RP2040 USB Host**](https://www.adafruit.com/product/5723) or [**Waveshare RP2350-USB-A**](https://www.waveshare.com/rp2350-usb-a.htm).
- A USB data cable from the adapter's **USB-C port to a Windows PC**. The keyboard plugs into the adapter's **USB-A host port**.
- **Microsoft Edge or Google Chrome on Windows** for the included HTML tuner.

## 1. Make the keyboard's four keys control its left stick

Set this up in your keyboard's own software *before* moving the keyboard to the adapter. Select its Gamepad/XInput mode, assign analog left-stick directions, and save an onboard profile:

| Key | Left-stick direction |
| --- | --- |
| **W** | Up / forward |
| **A** | Left |
| **S** | Down / backward |
| **D** | Right |

This beginner guide covers a keyboard that already sends an analog XInput left stick. WASD is an example; ESDF or another four-key layout works the same way when mapped in the keyboard's software. The layout is not hard-coded by the tuner: the adapter receives the keyboard's stick output. Check that light and deep presses produce different stick positions, not just on/off movement.

## 2. Flash the matching adapter firmware

When downloads are available, choose the package whose name exactly matches your board. Unzip it. Each board package contains one matching **.uf2** firmware file and **Analog Gamepad Helper.html**; keep them together. Keep your previous known-good UF2 if you have one.

1. Use a data-capable cable between the adapter's **USB-C device port** and the PC. Connect it as described for your board below.
2. Enter the board's UF2 bootloader:
   - **Feather RP2040 USB Host:** with USB-C unplugged, hold **BOOT** while connecting it to the PC; keep holding until the **RPI-RP2** drive appears. If already connected, hold **BOOT**, tap **RESET**, and release BOOT when the drive appears. [Adafruit's board guide](https://learn.adafruit.com/adafruit-feather-rp2040-with-usb-type-a-host/pinouts)
   - **Waveshare RP2350-USB-A:** hold **BOOT** while reconnecting USB-C, or hold BOOT and tap **RESET**; release RESET first, then BOOT. A removable boot drive appears. [Waveshare's board guide](https://www.waveshare.com/wiki/RP2350-USB-A)
3. Copy **only the .uf2 for your exact board** to that removable drive. The drive disappearing and the adapter restarting are normal.
4. Plug the configured keyboard into the adapter's **USB-A host port**. With the adapter connected directly to the PC, confirm that moving WASD moves a left stick on the resulting controller.

Do not copy the HTML file to the bootloader drive. It stays on your PC.

## 3. Open the tuner

Open **Analog Gamepad Helper.html from the same board package** in Edge or Chrome. It is a local file; it does not need a website or account.

1. Click **Link to hardware**.
2. If the adapter is currently a gamepad, follow the guided left-stick movement check to identify it. The browser may briefly disconnect that controller to switch it into configuration (COM) mode. Select the serial port only when the browser asks.
3. If the adapter is **already in COM mode**, choose **Already in COM mode — select port** instead. Do not send the gamepad-mode trigger again.
4. The tuner checks the Humanizer's identity before enabling settings. If more than one controller or serial port could be yours, stop and disconnect the extras rather than guessing.

Changes you make while linked take effect immediately in the adapter's working memory. **Save to device** keeps them after power loss and reboots the adapter. **Exit Without Saving** discards unsaved changes and returns to the saved configuration. **Reset All Values** changes the current working values; use Save to device only if you want that reset to persist.

**Bypass Humanizer** temporarily compares the keyboard's reported left stick without Humanizer processing. In Live Preview, *incoming* means that reported stick value before Humanizer, not the keyboard sensor's physical raw data. The COM preview's display rate is not a measurement of the PC's gamepad polling rate.

## 4. Tune one thing at a time

Start with **Reset All Values** and move WASD slowly, then in rolls. Add one effect, compare it with Bypass, and keep only what feels helpful. Some controls are experimental and can add noticeable delay. Reset All changes working values but does not save them until you choose **Save to device**.

The descriptions below follow the current Humanizer Studio controls. **Details** in the tuner explain the advanced controls further.

### Response and return

**Response Model.** Chooses the style of smoothed movement. Exponential: Makes the stick move smoothly toward the direction and distance you’re pressing. It moves quickly at first, then slows as it gets closer. Higher smoothing makes it take longer to reach that position. Guarded Inertia: Makes sudden stick movements more gradual and helps direction changes flow around corners. The stick eases into changes rather than following each key movement immediately. Phase-Aware: Helps the stick follow a rounded path when you change direction, rather than cutting across toward center. This helps keep the stick at a steadier distance from center during turns. Higher smoothing makes it follow your changes more gradually. Default: Exponential. Guarded Inertia and Phase-Aware are experimental.

**Response Smoothing.** Smooths how the stick follows your key movements. Lower follows sooner; higher moves more gradually but takes longer to catch up. 0 turns Response smoothing off. Used only in Global mode. Both deflection-based modes ignore this value completely. Default: 0%.

**Smoothing Mode.** Global uses one Response Smoothing amount. **Deflection Based · Simple** uses Low, Medium, and Full deflection values. **Deflection Based · Advanced** has ten standard points plus up to ten custom points: click the graph or use **Add point**, drag to adjust, or enter exact values. Custom points can also move between their neighbours or be removed. **Reset Curve** restores the standard curve. Both deflection modes ignore the stored Global value completely; switching back to Global restores its use. These values change timing, not stick reach.

Global, deflection-based, and Return smoothing all use **0–100%**, with a true zero. 1% corresponds to a 1 ms nominal smoothing time constant; it is not a fixed pause or a promise to finish moving in 1 ms. The **Advanced Response** Simple/Advanced selection is separate and works with Global or either deflection mode.

**Return Model.** Uses the same model selected in Response. Return has its own smoothing amount; there is no separate Return model selector in this build.

**Return Smoothing.** Smooths the stick moving back toward center, whether you release the keys gradually or all at once. Lower returns sooner; higher returns more gradually. Uses the Response model, with its own smoothing amount. 0% turns Return smoothing off. Default: 0%.

### Circularity, Angular Offset, Arc, and Wander

**Circularity.** Changes how far the stick reaches in diagonal directions. Lower makes the outer edge rounder; higher gives the diagonals more reach, making the edge more square-like. 50 leaves the original shape unchanged. Default: 50%.


**Apply Circularity** switches only the outer-edge correction on or off and keeps your slider value. If the keyboard offers square or circular gamepad output, square gives Humanizer the widest adjustment range. Humanizer cannot recover stick reach already removed by the keyboard.

**Maximum Angular Offset.** Varies the stick’s starting direction by up to the angle you choose, to either side of the direction you press. Lower keeps it closer to your pressed direction; higher allows more variation. Range: 0–5°. At 5°, each new starting offset can be up to 5° either way; it is not always 5°. 0° is off. Default: 0°. Degrees describe stick direction, not how far a game camera turns.

**Outward Arc.** Adds a varying bend as the stick moves away from center. Lower allows a smaller bend; higher allows a larger one. The current tuner displays the maximum bend in degrees, up to 5°. The actual bend varies and can be smaller. 0° adds no arc. Default: 0°. This limit describes the new arc, not all movement effects combined.

**Return Arc.** Adds a varying bend as the stick moves back toward center. Lower allows a smaller bend; higher allows a larger one. The current tuner displays the maximum bend in degrees, up to 5°. The actual bend varies and can be smaller. 0° adds no arc. Default: 0°. This limit describes the new arc, not all movement effects combined.

**Wander Amount.** Adds small changes in direction while you hold the keys steady. Lower makes the movement smaller; higher lets the direction wander farther. 0 is off. Default: 0%.

**Wander Speed.** Controls how quickly the direction wanders while you hold the keys steady. Lower makes the changes slower and less frequent. Higher makes them quicker and more frequent. Only has an effect when Wander Amount is above zero. 0% turns wandering off. Default: 0%.

### Advanced Response and Thumb Emulation

Response has model-specific **Advanced Response** controls, available under Global and both deflection modes.

On the **Thumb Emulation** page, select **Enable Thumb Emulation** and raise **Strength** to try it. It starts off at 0%. Simple always uses its fixed preset; Advanced restores your six custom values. Strength is shared between modes. **Reset Thumb Emulation** turns it off, restores Strength to 0%, selects Simple, and restores all six defaults without resetting other tuning.

**Start Shape — Exponential Response only.** Changes how the stick leaves center. Lower makes it move quickly at first, then ease into the position you’re pressing toward. Higher makes it start gently, then catch up. Default: 50%—matches Simple mode’s start shape.

**Turn Momentum — Guarded Inertia Response only.** Changes how the stick turns when you change direction. Lower: The stick turns toward the new direction more directly. Higher: The stick keeps traveling in its previous direction briefly as it turns toward the new one, making the turn rounder. Default: 100%—matches Simple mode’s turn momentum.

**Path Bias — Phase-Aware Response only.** Changes whether the stick cuts across a turn or follows a rounded path. Lower lets it cut closer to center to reach the new direction. Higher favors a rounder turn that stays farther from center. Default: 50%—matches Simple mode’s way of choosing the turning path.

**Strength.** Controls how much Thumb Emulation changes the stick’s movement. Lower follows your key direction and pressure more directly. Higher adds more rounding and distance stabilization during turns, but can respond more slowly to your changes. 0% turns all Thumb assistance off. Default: 0%.

**Turn Connection.** Softens changes in the stick’s direction during a turn. Lower follows a new direction more sharply. Higher makes the change more gradual and rounded, but takes longer to catch up. 0 ms removes this extra direction smoothing. Default: 20 ms—matches the Simple preset.

**Roll Persistence.** Keeps Thumb assistance active through brief pauses in a turn. Lower ends assistance sooner after the direction stops changing, letting the stick settle onto the position your keys request. Higher connects longer pauses, but can delay settling onto a held position. 0 ms removes this pause allowance. Default: 750 ms—matches the Simple preset.

**Radius Stability.** Steadies the stick’s distance from center while you turn, helping the path keep a consistent size. Lower follows deeper or lighter key presses sooner. Higher resists changes in distance more strongly. Higher settings strongly favor steady circles: at high Thumb Strength, intentional distance changes can take several seconds to follow. Default: 750 ms—matches the Simple preset.

**Circle Radius Compression.** Brings wider turning paths closer to center. Lower preserves more outward reach. Higher pulls wider paths farther inward, leaving more space before the outer edge. 0% turns this extra inward pull off. Default: 25%—matches the Simple preset.

**Hold Settle.** Eases the stick’s final change in direction after Thumb assistance ends during a pause. Lower aligns with the direction you are holding sooner. Higher makes that final alignment more gradual. 0 ms adds no extra settling. Default: 0 ms—matches the Simple preset.

**Full-Deflection Confirmation.** During an assisted turn, checks that an input asking for the outer edge is held long enough before letting the stick move out to it. Lower accepts that input sooner. Higher helps keep brief presses from throwing the stick out to the edge, but makes a sustained full-stick input take longer to follow. 0 ms removes this extra wait. Default: 40 ms—matches the Simple preset.

## 5. Compare the live previews

While linked in COM mode, Live Preview receives paired incoming and processed stick positions from the adapter.

| View | What it shows |
| --- | --- |
| **Processed only** | The stick movement after Humanizer. |
| **Overlay** | Incoming and tuned paths on the same plot, making their differences easy to see. |
| **Side by side** | Separate plots and X/Y, magnitude, and angle readouts for incoming and tuned movement. |

**Raw input** means the keyboard's reported stick before Humanizer. It is not a direct measurement of the individual key sensors. **Tuned output** is the processed stick inside the adapter. In COM mode this is serial preview data, not a simultaneous XInput endpoint.

Use **Expand preview** for a larger view or **Pop out** for a separate resizable window. Under **Preview appearance**, each trace has independent **Lines / Dashes / Dots**, color, thickness, and opacity. Trail fade is **1, 5, or 10 seconds**. **Clear trail** removes the visible history; **Reset appearance** restores the display defaults. These controls do not change movement tuning.

The current tuner automatically requests **1 ms COM preview** from compatible firmware. The browser draws at its own refresh rate; 1 ms preview does not mean 1,000 visible frames per second or prove a particular gamepad polling rate.

To compare an effect, keep the same movement and settings, use Overlay or Side by side, then toggle **Bypass Humanizer**. Bypass keeps your chosen effect settings so you can turn processing back on without rebuilding the configuration.

## 6. Save and share profiles

There are three separate ways to keep settings:

| Action | Where it saves | What it does to the adapter |
| --- | --- | --- |
| **Save current settings to browser** | A named preset in this browser | Does not save device flash. |
| **Save config** | A JSON file you can back up or share | Does not save device flash. |
| **Save to device** | The adapter's persistent storage | Saves the current settings and reboots into controller mode. |

Use **Load config** to open a shared JSON file. Selecting a browser preset or importing a config loads it into Studio and applies it live when linked. Use **Save to device** afterward if you want it to survive power loss.

For portable profiles, create a **Configs** folder beside the HTML and choose it in the save dialog. The browser needs your choice; it does not silently write beside the HTML. If a save dialog is unavailable, the file uses the browser's normal download location.

Supported older profiles are converted when loaded. A removed Thumb Motion or Inner Gate selection becomes off, rather than silently enabling Thumb Emulation. Review any migration notice before saving a new copy; the original imported file is preserved.

## 7. Check timing and record movement

Open **Diagnostics** while linked:

- **Press & release timing:** start centered, press and hold one direction fully, then release when prompted. Separate graphs show incoming and processed magnitude, endpoint markers, and a labeled bracket for the extra time to full magnitude or center.
- **Record stick movement:** capture paired incoming/processed samples, timing, and current settings to a local JSON file. Use **Stop & Save** to keep it. Recording stops after 60 seconds, on a tuning change, or if the connection is lost.

The timing test measures added settling time inside Humanizer with your current settings. It does not measure total keyboard-to-screen input lag. It uses device sample times, treats 99% magnitude as full and 1% as centered, and checks that each endpoint stays reached for 20 ms without adding that confirmation wait to the result. The test does not change your tuning.

## If something does not connect

- Confirm that the keyboard is in its analog **Gamepad/XInput** mode and that its WASD left-stick mapping is saved onboard.
- Confirm that the keyboard is in the adapter's **USB-A** port and the adapter's **USB-C** data cable goes directly to the PC.
- Use Edge or Chrome, open the HTML from the **same package** as the flashed firmware, and grant serial access only to the Humanizer port you identify.
- If the adapter is already in COM mode, use that route rather than trying to trigger the mode switch again.
- If you are unsure which controller or port is yours, disconnect other controllers and retry identification. Do not guess from a numbered XInput slot.
