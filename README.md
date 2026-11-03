# Crash Cart

Crash Cart turns a Flipper Zero into a small **USB keyboard** for a server's
firmware screens. Each of the six buttons (Up, Down, Left, Right, OK, Back)
sends one key or key combo, and named **profiles** hold the assignments: one
profile per machine for its POST hotkeys, and a generic one for moving around
setup menus. Plug the Flipper into the server with a USB-C cable and you don't
have to go and find a keyboard.

A BIOS can't use a Bluetooth keyboard, so Crash Cart is USB only.

Crash Cart is a USB port of [Anki Remote](https://github.com/Blue5GD/Anki-Remote)
by Blue5GD, ported from upstream commit
[`a47c09b`](https://github.com/Blue5GD/Anki-Remote/commit/a47c09b8f8693f7a1e6e5046cec78dbd6050c65b)
(2026-06-12). Anki Remote did the hard parts: per-button mapping of any key or
combo, named presets on the SD card and the on-device key picker.

## Using it

Open **Crash Cart**, choose **Start**, and plug the Flipper into the machine.
The Flipper becomes a USB keyboard as soon as the app opens and goes back to
being a serial device when you quit it. The blue LED is on while the host has
the keyboard connected.

On the controller screen the header shows the active profile, so you can always
see which keys are live.

| Button | Does |
|---|---|
| Up, Down, Left, Right, OK | Send their keys. Holding one holds the key, so the host repeats it. |
| Back, tap | Sends Back's key. While the screen says "Awaiting USB", goes to the main menu instead. |
| Back, hold | Opens the profile switcher. Sends nothing. |

In the **profile switcher**, Up and Down move, **OK** makes the highlighted
profile active (and remembers it for next time) and returns to the controller,
and **Back** goes to the main menu. Back again from there quits the app.

### Default profiles

Two profiles are created on first run, with **Dell POST** active.

**Dell POST**: the POST hotkeys of recent Dell PowerEdge servers.

| Button | Key | On a PowerEdge |
|---|---|---|
| Left | F1 | Continue past a "Strike the F1 key" stop |
| Up | F2 | System Setup |
| OK | F10 | Lifecycle Controller |
| Right | F11 | Boot Manager |
| Down | F12 | PXE boot |
| Back (tap) | Ctrl+R | PERC RAID configuration utility |

With UEFI boot the PERC's Ctrl+R prompt may not appear. Its settings are then
under F2 → Device Settings.

**Navigate**: for any setup menu. The d-pad sends the arrow keys, OK sends
Enter and a Back tap sends Esc.

So to get into a PowerEdge's setup: reboot it with Dell POST active, tap Up for
F2, then hold Back → Down → OK to switch to Navigate.

### Managing profiles

**Manage Profiles** in the main menu lists the profiles (the active one is
starred) and can edit, rename, delete and create them. Up to 12 profiles, with
names of up to 16 characters. Selecting a profile and pressing OK makes it
active; **Edit Selected**, **Rename Selected** and **Delete Selected** act on the
active profile. Editing a button opens the key picker: toggle Shift, Ctrl, Alt
or Cmd first to build a combo, or use the bottom row to send a modifier on its
own. **LED Brightness** sets the connection LED.

Profiles are saved to `/ext/apps_data/crash_cart/presets.dat` on the SD card.

## Building

Crash Cart is built with [ufbt](https://github.com/flipperdevices/flipperzero-ufbt)
against the SDK that matches the firmware on your Flipper; a mismatch makes the
app refuse to launch. For official release firmware:

```sh
pipx install ufbt          # or: uv tool install ufbt
ufbt update --channel=release
ufbt                       # builds dist/crash_cart.fap
ufbt launch                # uploads and runs it on a connected Flipper
ufbt lint                  # clang-format check
```

While the app is running the Flipper is a USB keyboard, not a serial device,
so `ufbt launch` and `ufbt cli` can't reach it. Quit the app first (hold Back,
then Back, then Back again from the main menu). If it ever gets stuck as a
keyboard, reboot the Flipper by holding Left and Back.

CI builds the app against the official release and dev SDK channels on every
push.

## Testing

`tools/keytest.html` is a static page that logs every `keydown` and `keyup`
with its `event.code` and modifier state, collapses auto-repeat into a count,
and shows any key that went down without coming back up. Open it in a browser,
launch the app with the Flipper plugged into the same computer, and press the
buttons. macOS keeps some F-keys for itself (F11 is Show Desktop by default),
so an F-key that doesn't show up there isn't necessarily a bug.

## Licence

GPL-3.0, like Anki Remote. See [LICENSE](LICENSE).

## Anki Remote's credits

Anki Remote credits these projects, and Crash Cart still builds on them:

- [Flipper Zero's Official HID](https://github.com/flipperdevices/flipperzero-firmware/tree/dev/applications/system/hid_app) keyboard logic, core functionality, most images, and the remote screen layout.
- [BT Trigger](https://github.com/xMasterX/all-the-plugins/tree/dev/apps_source_code/bluetooth-trigger) bluetooth cross-compatibility, the awaiting bluetooth screen layout, and general example code.
- [Instantiator's Flipper Zero App Tutorial](https://instantiator.dev/post/flipper-zero-app-tutorial-01/) installing ufbt and setting up the development environment in VSCode.
