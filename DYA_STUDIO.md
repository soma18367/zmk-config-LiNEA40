# DYA Studio rollout

`main` remains the stable firmware source. `sync-device-layout` records the seven layers displayed by the connected LiNEA40 in DYA Studio on 2026-10-07. The pending Studio changes were saved, and all seven layers were read back after a reload. `dya-studio` carries the same keymap while extension work proceeds.

## Stage 1: existing keymap support

The repository already has the official ZMK Studio integration used by DYA Studio's Keymap screen:

- `LiNEA40.dtsi` defines the physical layout, its keys, and the matrix transform.
- `build.yaml` enables `studio-rpc-usb-uart` and `CONFIG_ZMK_STUDIO=y` on the right half, which is the central half connected to USB.
- `LiNEA40.keymap` has `&studio_unlock` on Layer 6.

USB connection and the seven-layer Keymap screen were confirmed on the device. The `dya-studio` build at `06f9904` passed all three build jobs. Its right-half UF2 was copied to the right half's XIAO-SENSE bootloader volume. The volume disappeared as the keyboard rebooted, and DYA Studio reconnected to LiNEA40. Studio showed "saved" and the expected Layer 0 and Layer 4 bindings after reconnecting. The copy command reported an extended-attributes error during auto-unmount, but the user subsequently confirmed that the newly added `CtrlKey` combo works on the keyboard. The device does not expose a firmware version through the current Studio diagnostics.

The user also confirmed the following post-flash behavior: Layer 0 Esc/Command tap-hold, Caps, Backslash tap/right Ctrl hold; Layer 4 range and full-screen screenshots and Globe. These checks cover the requested keymap changes. Trackball and Bluetooth behavior after flashing have not been separately reported.

All existing static combos, including `CtrlKey`, remain in the firmware source. DYA Studio cannot display or edit the static combos through its runtime combo screen with the current firmware. No new DYA module or firmware revision is enabled in this stage. The left half and settings-reset target were not flashed.

## Next stages

1. Confirm trackball, Bluetooth, and Studio unlock after flashing before changing firmware modules.
2. Add one DYA extension at a time, starting with a feature that does not replace an existing input path. Pin and review any new ZMK or module revision, then build both halves and the settings-reset target.
3. Compare the keymap and static combos against `sync-device-layout` after every stage.

Runtime combo editing requires cormoran's custom Studio Protocol fork and custom-settings module. Trackball editing requires changes to the PMW3610 driver and input processing. Keep these as separate changes until their compatibility and device behavior are verified. ZMK Studio saves keymap edits in the keyboard's settings; after flashing a new stock keymap, use the Studio restore action only after checking the saved layout.

## Runtime combo build (2026-10-10)

Branch `dya-runtime-combo` enables runtime combos on the central/right half.
The custom Studio ZMK fork, custom-settings module and runtime-combo module
are pinned to reviewed commits in `config/west.yml`. The custom Studio fork uses Zephyr 4.1 and the modern
`xiao_ble/nrf52840/zmk` board target; the build workflow is pinned to a
compatible revision. Build and device checks are required before flashing.

The eight static combos are preserved as compile-time runtime defaults on
the central half (slots 0–7); slots 8–15 are available for new combos. Their
positions, bindings and layer restrictions are unchanged. The peripheral
build retains the original static definitions. Normal key bindings, encoder
bindings and trackball tuning are unchanged. The original PMW3610 driver
is vendored with a distinct compatible name to avoid the new Zephyr binding;
RGB LED aliases use the same physical pins. Macro editing is not
enabled by this change.

Before flashing, save/export the current device keymap and keep the previously
working UF2 files. Do not flash the settings-reset image or restore stock
settings as part of the upgrade. The changed ZMK fork is built for both halves;
flash matching left/right UF2 files and verify Bluetooth, trackball, encoder
and Studio unlock. Connect DYA Studio, check the eight default combos, edit
one combo, save to flash, reconnect and verify it persisted. Actual device
validation remains pending until reported by the user.
