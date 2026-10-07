# DYA Studio rollout

`main` remains the stable firmware source. `sync-device-layout` records the seven layers displayed by the connected LiNEA40 in DYA Studio on 2026-10-07. The editor showed "unsaved changes", so the displayed layout must be checked against the keyboard's persisted layout before flashing or resetting settings. `dya-studio` carries the same keymap while extension work proceeds.

## Stage 1: existing keymap support

The repository already has the official ZMK Studio integration used by DYA Studio's Keymap screen:

- `LiNEA40.dtsi` defines the physical layout, its keys, and the matrix transform.
- `build.yaml` enables `studio-rpc-usb-uart` and `CONFIG_ZMK_STUDIO=y` on the right half, which is the central half connected to USB.
- `LiNEA40.keymap` has `&studio_unlock` on Layer 6.

USB connection and the seven-layer Keymap screen were confirmed on the device. All existing static combos, including `CtrlKey`, remain in the firmware source. DYA Studio cannot display or edit the static combos through its runtime combo screen with the current firmware. No new DYA module or firmware revision is enabled in this stage.

## Next stages

1. Build all three entries in `build.yaml` from `dya-studio`. Review the displayed keymap, then test on the keyboard before changing firmware modules.
2. Add one DYA extension at a time, starting with a feature that does not replace an existing input path. Pin and review any new ZMK or module revision, then build both halves and the settings-reset target.
3. Verify tap/hold keys, screenshots, Globe, `CtrlKey`, trackball, Bluetooth, and Studio unlock on the device. Compare the keymap and static combos against `sync-device-layout` after every stage.

Runtime combo editing requires cormoran's custom Studio Protocol fork and custom-settings module. Trackball editing requires changes to the PMW3610 driver and input processing. Keep these as separate changes until their compatibility and device behavior are verified. ZMK Studio saves keymap edits in the keyboard's settings; after flashing a new stock keymap, use the Studio restore action only after checking the saved layout.
