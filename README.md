# Analog Keyboard Humanizer

Turn an analog keyboard's left-stick output into more natural, adjustable controller movement. The adapter keeps your actual key depth and changes only the left stick. Buttons, triggers, and the right stick pass through unchanged.

> **Downloads are not published yet.** This page is the getting-started guide. When the board packages are ready, they will appear under [Releases](https://github.com/Rambosstic/Analog-Keyboard-Humanizer-/releases). Do not use a firmware file meant for the other board.

## What you need

- An analog keyboard that can output a real **left stick in Gamepad/XInput mode** and save that mapping for use without its desktop software. Four ordinary digital WASD keys do not become pressure-sensitive just by connecting this adapter.
- One supported adapter: **Adafruit Feather RP2040 USB Host** or **Waveshare RP2350-USB-A**.
- A USB data cable from the adapter's **USB-C port to a Windows PC**. The keyboard plugs into the adapter's **USB-A host port**.
- **Microsoft Edge or Google Chrome** for the included HTML tuner. This guide uses the browser's Web Serial connection.

Start with a direct PC connection. Other hosts and intermediary devices are not part of this beginner setup.

## 1. Make the keyboard's four keys control its left stick

Set this up in your keyboard's own software *before* moving the keyboard to the adapter. Select its Gamepad/XInput mode, assign analog left-stick directions, and save an onboard profile:

| Key | Left-stick direction |
| --- | --- |
| **W** | Up / forward |
| **A** | Left |
| **S** | Down / backward |
| **D** | Right |

This beginner guide covers a keyboard that already sends an analog XInput left stick. WASD is an example, not a setting hard-coded by the tuner: the adapter receives the keyboard's stick output. Check that light and deep presses produce different stick positions, not just on/off movement.

## 2. Flash the matching adapter firmware

When downloads are available, choose the package whose name exactly matches your board. Unzip it. Each board package contains one matching **.uf2** firmware file and **Analog Gamepad Helper.html**; keep them together. Keep your previous known-good UF2 if you have one.

1. Connect the adapter's **USB-C device port** to the PC with a data-capable cable.
2. Enter the board's UF2 bootloader:
   - **Feather RP2040 USB Host:** hold **BOOT**, tap **RESET**, and release BOOT when the **RPI-RP2** drive appears. [Adafruit's board guide](https://learn.adafruit.com/adafruit-feather-rp2040-with-usb-type-a-host?view=all)
   - **Waveshare RP2350-USB-A:** hold **BOOT** while reconnecting USB-C, or hold BOOT and tap **RESET**; release RESET first, then BOOT. A removable boot drive appears. [Waveshare's board guide](https://www.waveshare.com/wiki/RP2350-USB-A)
3. Copy **only the .uf2 for your exact board** to that removable drive. The drive disappearing and the adapter restarting are normal.
4. Plug the configured keyboard into the adapter's **USB-A host port**. With the adapter connected directly to the PC, confirm that moving WASD moves a left stick on the resulting controller.

Do not copy the HTML file to the bootloader drive. It stays on your PC.

## 3. Open the tuner

Open the **Analog Gamepad Helper.html from the same board package** in Edge or Chrome. It is a local file; it does not need a website or account.

1. Click **Link to hardware**.
2. If the adapter is currently a gamepad, follow the guided left-stick movement check to identify it. The browser may briefly disconnect that controller to switch it into configuration (COM) mode. Select the serial port only when the browser asks.
3. If the adapter is **already in COM mode**, choose **Already in COM mode — select port** instead. Do not send the gamepad-mode trigger again.
4. The tuner checks the Humanizer's identity before enabling settings. If more than one controller or serial port could be yours, stop and disconnect the extras rather than guessing.

Changes you make while linked take effect immediately in the adapter's working memory. **Save to device** keeps them after power loss and reboots the adapter. **Exit Without Saving** discards unsaved changes and returns to the saved configuration. **Reset All Values** changes the current working values; use Save to device only if you want that reset to persist.

**Bypass Humanizer** temporarily compares the keyboard's reported left stick without Humanizer processing. In Live Preview, *incoming* means that reported stick value before Humanizer, not the keyboard sensor's physical raw data. The COM preview's display rate is not a measurement of the PC's gamepad polling rate.

## 4. Tune one thing at a time

Start with **Reset All Values** and move WASD slowly, then in rolls. Add one effect, compare it with Bypass, and keep only what feels helpful. Some controls are experimental and can add noticeable delay. Reset All changes working values but does not save them until you choose **Save to device**.

The descriptions below use the labels and movement wording shown by the matching tuner's Feel-capable interface. **Details** in the tuner explain the advanced controls further.

### Response and return

**Response Model.** Chooses the style of smoothed movement. Exponential: Makes the stick move smoothly toward the direction and distance you’re pressing. It moves quickly at first, then slows as it gets closer. Higher smoothing makes it take longer to reach that position. Guarded Inertia: Makes sudden stick movements more gradual and helps direction changes flow around corners. The stick eases into changes rather than following each key movement immediately. Phase-Aware: Helps the stick follow a rounded path when you change direction, rather than cutting across toward center. This helps keep the stick at a steadier distance from center during turns. Higher smoothing makes it follow your changes more gradually. Default: Exponential. Guarded Inertia and Phase-Aware are experimental.

**Response Smoothing.** Smooths how the stick follows your key movements. Lower follows sooner; higher moves more gradually but takes longer to catch up. 0 turns Response smoothing off. Used only in Global mode. Both deflection-based modes ignore this value completely. Default: 0%.

**Smoothing Mode.** Deflection Based Smoothing replaces the global Response Smoothing value. Its Simple and Advanced points span 0–200%; 0% responds immediately and values from 1–9% scale proportionally. In either deflection-based editor, set smoothing directly for light, medium and full stick movement, or choose the six-point curve for finer control. Lower values follow your keys faster; higher values move more gradually. 0 adds no Response smoothing at that point. The Global slider is not used at all. These values change timing, not stick reach.

**Return Model.** Chooses the style of movement as the stick comes back toward center. Choose independently from Response: for example, Phase-Aware Response with Exponential Return. Your choices stay selected until you change them. When you turn while easing off, both choices can influence the movement. Default: Exponential.

**Return Smoothing.** Smooths the stick moving back toward center, whether you release the keys gradually or all at once. Lower returns sooner; higher returns more gradually. Uses your chosen Return Model, with its own smoothing amount. 0% turns Return smoothing off. Default: 0%.

### Motion Character

**Circularity.** Changes how far the stick reaches in diagonal directions. Lower makes the outer edge rounder; higher gives the diagonals more reach, making the edge more square-like. 50 leaves the original shape unchanged. Default: 50%.

**Maximum Angular Offset.** Varies the stick’s starting direction by up to the angle you choose, to either side of the direction you press. Lower keeps it closer to your pressed direction; higher allows more variation. Range: 0–5°. At 5°, each new starting offset can be up to 5° either way; it is not always 5°. 0° is off. Default: 0°. Degrees describe stick direction, not how far a game camera turns.

**Outward Arc.** Adds a varying bend as the stick moves away from center. Lower allows a smaller bend; higher allows a larger one. 50% allows each new arc to add up to 2.5°; 100% allows up to 5°. The actual bend varies and can be smaller. 0% adds no arc. Default: 0%. These limits describe the new arc, not all movement effects combined.

**Return Arc.** Adds a varying bend as the stick moves back toward center. Lower allows a smaller bend; higher allows a larger one. 50% allows each new arc to add up to 2.5°; 100% allows up to 5°. The actual bend varies and can be smaller. 0% adds no arc. Default: 0%. These limits describe the new arc, not all movement effects combined.

**Wander Amount.** Adds small changes in direction while you hold the keys steady. Lower makes the movement smaller; higher lets the direction wander farther. 0 is off. Default: 0%.

**Wander Speed.** Controls how quickly the direction wanders while you hold the keys steady. Lower makes the changes slower and less frequent. Higher makes them quicker and more frequent. Only has an effect when Wander Amount is above zero. 0% turns wandering off. Default: 0%.

### Customize the feel — Experimental

Response has model-specific Advanced controls. Thumb Emulation has Simple and Advanced modes: **Thumb Strength** is shared, while the timing and shape controls below customize Advanced. The tuner keeps unused mode values for later; only the selected mode affects movement.

**Start Shape — Exponential Response only.** Changes how the stick leaves center. Lower makes it move quickly at first, then ease into the position you’re pressing toward. Higher makes it start gently, then catch up. Default: 50%—matches Simple mode’s start shape.

**Turn Momentum — Guarded Inertia Response only.** Changes how the stick turns when you change direction. Lower: The stick turns toward the new direction more directly. Higher: The stick keeps traveling in its previous direction briefly as it turns toward the new one, making the turn rounder. Default: 100%—matches Simple mode’s turn momentum.

**Path Bias — Phase-Aware Response only.** Changes whether the stick cuts across a turn or follows a rounded path. Lower lets it cut closer to center to reach the new direction. Higher favors a rounder turn that stays farther from center. Default: 50%—matches Simple mode’s way of choosing the turning path.

**Thumb Strength.** Makes turns between key directions flow more like rolling a thumbstick. Lower follows your key changes more directly; higher makes rounder, more gradual turns but may take longer to follow your changes. 0 is off. Default: 0%.

**Turn Connection.** Smooths the change from one direction to another during a turn. Lower turns more sharply; higher rounds out the turn but follows your keys more slowly. Default: 20 ms—matches the Simple preset.

**Roll Persistence.** Keeps a turn going through brief pauses between key changes. Lower stops the turning effect sooner; higher connects longer pauses but can take longer to settle into a held direction. Default: 1,500 ms—matches the Simple preset.

**Radius Stability.** Controls how strongly the stick holds its circle size while you keep turning. Lower lets the circle grow or shrink sooner when you press deeper or lighter. Higher resists those changes more strongly. The 3,000 ms default strongly favors steady circles: a deliberate change in circle size can take several seconds to follow. Default: 3,000 ms—matches the Simple preset.

**Circle Radius Compression.** Makes larger circles smaller, bringing the stick closer to center. Lower keeps more outward reach; higher pulls the circle farther inward. 0 turns this extra inward pull off. Default: 25%—matches the Simple preset.

**Hold Settle.** Softens the final move into a held direction after a turn ends. Lower settles into that direction sooner; higher eases into it more slowly. 0 adds no extra settling time. Default: 0 ms—matches the Simple preset.

**Full-Deflection Confirmation.** Helps prevent the stick from jumping to the outer edge when you briefly press a key all the way down during a turn. Lower accepts full pressure sooner. Higher waits longer to check that you are holding full pressure, so reaching the edge can take longer. 0 adds no waiting time. Default: 80 ms—matches the Simple preset.

**Handoff Dip.** Briefly brings the stick closer to center when changing between directions during a turn. Lower makes a smaller dip; higher makes a deeper dip. 0 is off. Default: 0%—matches the Simple preset.

## If something does not connect

- Confirm that the keyboard is in its analog **Gamepad/XInput** mode and that its WASD left-stick mapping is saved onboard.
- Confirm that the keyboard is in the adapter's **USB-A** port and the adapter's **USB-C** data cable goes directly to the PC.
- Use Edge or Chrome, open the HTML from the **same package** as the flashed firmware, and grant serial access only to the Humanizer port you identify.
- If the adapter is already in COM mode, use that route rather than trying to trigger the mode switch again.
- If you are unsure which controller or port is yours, disconnect other controllers and retry identification. Do not guess from a numbered XInput slot.

## Scope

This is an external USB-host adapter, not keyboard firmware. It does not create analog depth that the keyboard does not report. It does not change unrelated controls or make a keyboard appear as an authenticated Xbox One/Series controller. This first setup and tuner workflow is for Windows PCs and the keyboard's native Gamepad/XInput left-stick path. Advanced raw-HID integrations are outside this beginner guide.

