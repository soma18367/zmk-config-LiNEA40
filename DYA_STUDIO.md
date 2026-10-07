# DYA Studio rollout

This branch starts from `sync-device-keymap`. `main` remains the stable firmware source.

## Stage 1: existing keymap support

The repository already has the official ZMK Studio integration used by DYA Studio's Keymap screen:

- `LiNEA40.dtsi` defines the physical layout, its keys, and the matrix transform.
- `build.yaml` enables `studio-rpc-usb-uart` and `CONFIG_ZMK_STUDIO=y` on the right half, which is the central half connected to USB.
- `LiNEA40.keymap` has `&studio_unlock` on Layer 6.

The synchronized keymap and all existing combos, including `CtrlKey`, are the baseline. No DYA module or firmware revision is changed in this stage. Build all three entries in `build.yaml` before flashing. Confirm the Keymap screen and the physical layout on the device before enabling another module.

## Next stages

1. Add one DYA extension at a time on this branch, starting with a feature that does not replace an existing input path.
2. Pin and review any new ZMK or module revision, then build both halves and the settings-reset target. Compare the keymap and static combos against `sync-device-keymap`.
3. Test the firmware on the keyboard and verify that tap/hold keys, screenshots, Globe, `CtrlKey`, trackball, Bluetooth, and Studio unlock still work.

Runtime combo editing requires cormoran's custom Studio Protocol fork and custom-settings module. Trackball editing requires changes to the PMW3610 driver and input processing. Keep these as separate changes until their compatibility and device behavior are verified. ZMK Studio saves keymap edits in the keyboard's settings; after flashing a new stock keymap, use the Studio restore action only after checking the saved layout.
