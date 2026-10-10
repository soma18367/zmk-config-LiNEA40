Vendored from https://github.com/inorichi/zmk-pmw3610-driver at
1d9c2c68ca76012e1b1e5f6ef02fa5eadc4ca399. Source SPDX notices retained.

Local change: rename the device-tree compatible to `inorichi,pmw3610`
to avoid conflicting with Zephyr 4.1's built-in `pixart,pmw3610` binding.
The driver algorithms and tuning are unchanged.
