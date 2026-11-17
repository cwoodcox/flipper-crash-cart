# Crash Cart

## v1.0
- USB port of Anki Remote 1.3: the Flipper is a USB keyboard while the app runs, instead of Bluetooth LE
- Back: a tap sends its key, a hold opens a new profile switcher (it used to send its key on press, then exit on hold)
- "Presets" are now "profiles"; names can be up to 16 characters
- Two default profiles: Dell POST (PowerEdge POST hotkeys) and Navigate (arrows, Enter, Esc)
- Keys are released whenever the controller screen is entered or left, and releases retry if the host is slow to collect them
- Built for official firmware 1.5.1 (API 88.2). The key picker's Sel key needs 1.5.1 or later: older firmware's keyboard descriptor stopped at key code 0x65, so hosts ignored it

# Anki Remote (upstream)

## v1.3
- added presets, reworked app functionality
- added new keyboard buttons (PrtSc, ScrLk, many more) 
## v1.2
- added option to adjust LED light in settings
## v1.1
- bug fixes, removed default keymaps, added "reset keymaps" option
- updated README and screenshots to reflect changes
## v1.0 
- released app
