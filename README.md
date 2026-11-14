# Analog Keyboard Humanizer

### Make analog keyboard movement feel more like a thumbstick.

Analog keys give you control over how far you move. Getting four separate
fingers to move together as smoothly as one thumb is the tricky part.

Humanizer sits between your analog keyboard and PC, letting you soften presses
and releases, shape turns, and explore more fluid movement inside the stick's
outer edge. You choose how much help you want, and you can watch the difference
as you tune.

It works with the keyboard's existing **XInput left-stick output**. Your analog
key depth still matters. Buttons, triggers, and the right stick pass through
unchanged.

**[Releases and downloads](https://github.com/Rambosstic/Analog-Keyboard-Humanizer-/releases)** ·
**[Report a problem or share feedback](https://github.com/Rambosstic/Analog-Keyboard-Humanizer-/issues)**

*Board packages are coming to Releases. Each package will include the firmware
and its matching Humanizer Studio configurator. This repository is the home for
downloads and user guides; firmware source code is not distributed here.*

## What you'll need

- An **analog keyboard with XInput gamepad mode**, capable of saving its
  left-stick mapping onboard. DirectInput and ordinary digital keyboard mode
  aren't supported.
- Either supported board:
  - [**Waveshare RP2350-USB-A**](https://www.waveshare.com/rp2350-usb-a.htm)
  - [**Adafruit Feather RP2040 USB Host**](https://www.adafruit.com/product/5723)
- A Windows PC, a USB data cable, and **Chrome or Edge** for the configurator.

The connection is simple:

**Keyboard → board's USB-A port → board's USB-C port → PC**

## Get started

### 1. Set up your keyboard

In your keyboard's own software, enable **XInput / Xbox controller mode** and
map four analog keys to the left stick. WASD is a familiar starting point:
W = up, A = left, S = down, D = right.

Save that setup to the keyboard. Before using the adapter, check that a light
press moves the stick a little and a deeper press moves it farther. Humanizer
receives that stick movement, so you can use a different key layout if you prefer.

**If your keyboard offers square or circular stick output, choose square.**
That gives Humanizer's Circularity control the most room to shape the outer edge.

### 2. Install the firmware for your board

Download and unzip the matching board package from **Releases**. Keep its
`.uf2` firmware and `Analog Gamepad Helper.html` together—use the configurator
that came with the firmware you're installing.

<details>
<summary><strong>How to install the firmware</strong></summary>

1. Connect the board's USB-C port to your PC using a data-capable cable.
2. Enter its bootloader:
   - **Waveshare RP2350-USB-A:** hold **BOOT**, tap and release **RESET**, then
     release BOOT. You can also hold BOOT while connecting USB-C.
   - **Adafruit Feather RP2040 USB Host:** hold **BOOT** while connecting
     USB-C, or hold BOOT and tap RESET if it's already connected.
3. A removable drive appears. Copy the **`.uf2` for your exact board** onto it.
4. The drive disappears as the board restarts. That's expected.
5. Plug your configured keyboard into the board's USB-A host port.

The HTML configurator stays on your PC; it doesn't go onto the bootloader drive.
Keep a copy of your previous working firmware when updating.

Board instructions: [Waveshare](https://www.waveshare.com/wiki/RP2350-USB-A) ·
[Adafruit](https://learn.adafruit.com/adafruit-feather-rp2040-with-usb-type-a-host/pinouts)

</details>

### 3. Open Humanizer Studio

Open **`Analog Gamepad Helper.html`** in Chrome or Edge, then click
**Link to hardware** and follow the prompts. Studio runs from that local file;
there's no account to create.

The connection guide identifies your adapter and helps switch it into tuning
mode. If it's already in COM mode, choose **Already in COM mode — select port**.
If several devices look alike, disconnect the extras so you can select yours
confidently.

Studio remembers your last setup in the same browser. If there isn't one yet,
it starts with **Clean Slate**. Linking reads the adapter's current settings
first. If they differ from your prepared Studio setup, choose **Use adapter
settings** or **Apply my Studio settings**. Nothing is saved to the adapter's
flash until you click **Save to device**.

Keep a saved config file as a portable backup—browser storage may not follow
you to another browser or a different copy of the HTML file.

### 4. Find your feel

Settings take effect while you're linked, so you can adjust a slider and try
the movement immediately. A useful first experiment is:

1. Start with **Reset All Values** for a clean starting point.
2. Choose **Exponential**, then try **10% Response** and **10% Return**.
3. Try quick presses, gradual releases, and rolling between neighboring keys.
4. Compare **Phase-Aware** at the same values, then adjust to taste.

Those values are just a starting point. More smoothing makes movement more
gradual and takes longer to follow your changes.

For a ready-made starting point, load **Profiles → Suggested Settings Basic**:

| Setting | Value |
| --- | --- |
| Circularity | On, 3% |
| Response | Exponential, Global 10% |
| Return | 10% |
| Maximum Angular Offset | 2° |
| Outward Arc / Return Arc | Up to 2° each |
| Wander Amount / Speed | 10% each |
| Thumb Emulation | On, Simple, Strength 5% |

This preset is optional. Load it when you want to try it, then adjust to taste.

### Start fresh whenever you like

**Reset All Values** clears your current tuning to a clean starting point:
smoothing, arc, offset, and wander amounts are zero, Thumb Emulation is off,
and Circularity is at 50% with **Apply Circularity unchecked**. All deflection
smoothing points are zero too. Inactive Advanced controls keep their normal
defaults, ready for when you choose to use them.

You can load that same starting point from **Profiles → Clean Slate**. It's
built into Studio and always available, even if you haven't saved any profiles.
Use **Save to device** if you want to keep this reset after unplugging.

## A quick tour of the tuning pages

| Page | What to explore |
| --- | --- |
| **Response** | How smoothly the stick follows your presses and direction changes. Choose Exponential, Guarded Inertia, or Phase-Aware. |
| **Return** | How smoothly it moves inward as you ease off the keys, including during a roll. It follows your selected Response model, with its own amount. |
| **Circularity** | The shape of the outer edge. Turn **Apply Circularity** on to use it; lower values make the boundary rounder. |
| **Angular Offset** | A little variation in the direction a movement starts. |
| **Arc** | A varying bend while moving outward or returning inward. |
| **Wander** | Small changes in direction while holding steady, with separate amount and speed controls. |
| **Thumb Emulation** | Extra help connecting turns and controlling their radius. It starts off; enable it and raise Strength to try it. |
| **All Tunings** | Every tuning section together, with Live Preview still available. |

Each control has its own explanation in Studio, so you don't need to memorize
the settings before trying them.

**Want finer control?** Response's deflection-based smoothing lets you use
different amounts for light, medium, and full stick movement. Its advanced curve
has ten standard points and room for ten custom points you can add and drag.
When using either deflection-based mode, the Global smoothing slider is disabled.

**Thumb Emulation** has a Simple preset and six Advanced controls. Simple always
uses its preset; Advanced brings back your custom values. Strong settings can
make changes in direction or circle size take noticeably longer, so try small
adjustments first. **Reset Thumb Emulation** resets just that section.

## See what your settings are doing

**Live Preview** shows the keyboard's incoming stick movement and Humanizer's
processed output. View them together, side by side, or show processed movement
alone. Expand the preview or pop it into its own window when you want more space.

In **Preview appearance**, choose colors, lines, dashes or dots, thickness,
opacity, and trail fade. These change the view, not your movement settings.

Under **Diagnostics**, the **Press & release timing** test puts numbers to the
feel: press and hold one direction, then release when prompted. It shows how
much extra time your settings take to reach full movement and return to center.
The graphs compare raw and processed movement, with the added delay labeled
between their markers.

That is **movement settling time inside Humanizer**, not total keyboard-to-screen
latency. “Raw” here means the stick output your keyboard reports before
Humanizer processes it.

## Keep a setup you like

- **Save to device** stores your current settings on the adapter and restarts it.
- **Exit Without Saving** returns to the last saved device settings.
- **Profiles** lets you save and load settings files, share them, or keep named
  presets in your browser.

Loading a profile and saving it to the adapter are separate steps. Once you've
found a setup you like, choose **Save to device** to keep it after unplugging.

## Need a hand?

If nothing moves, check the basics first: **XInput mode**, an onboard left-stick
mapping, keyboard in **USB-A**, and a data-capable **USB-C** connection to the PC.
Some keyboards take a few seconds to initialize; Turtle Beach can take longer
than Wooting.

If Studio won't link, try Chrome or Edge and make sure the HTML and firmware
came from the same release package. Close other applications using the adapter's
serial port before trying again.

For unusual movement, **Diagnostics → Record stick movement** saves a recording
you can attach to an issue. Include your board, keyboard, release version,
settings, and a short description of what you expected versus what happened.

Humanizer is still being refined. Feedback from real keyboards and real play
helps shape where it goes next.
