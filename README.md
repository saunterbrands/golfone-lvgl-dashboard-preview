# Golf One Dashboard LVGL Editor Project

This is the LVGL Editor source project for porting the Hole 4 browser dashboard into native LVGL elements.

Source of truth:

- HTML/CSS reference: `/Users/justinhendrickson/Downloads/Cart-Key-Branch-Lookup/esp32-small-screen/assets/courses/scenic-view/viewer/hole-4-dashboard.html`
- Firmware target: `/Users/justinhendrickson/Downloads/Cart-Key-Branch-Lookup/apps/waveshare-p4-nano-10-1-nano/main/main.cpp`

Current scope:

- `components/left_footer.xml` is the first real port. It maps the HTML `.left-footer-card` into LVGL objects with live subjects for pace and clock text.
- `components/pin_readout.xml` is intentionally standalone. The firmware should keep treating pin readout as its own element, not as part of a left or right rail crop.
- `screens/dashboard_target.xml` is an editor preview screen at the browser dashboard's 1024x600 design coordinate system.

The glass effect is approximated with LVGL layers:

- translucent shell object
- white top shine object
- dark lower shade object
- live labels/images above those layers

LVGL Editor does not give us browser `backdrop-filter`; the firmware still has to back the glass from the live Blender map/background pixels when running on hardware.

Useful commands once `lved-cli.js` is installed from LVGL Editor releases:

```bash
lved-cli.js validate /Users/justinhendrickson/Downloads/Cart-Key-Branch-Lookup/apps/waveshare-p4-nano-10-1-nano/tools/lvgl-editor-dashboard --errorlimit 25
lved-cli.js generate /Users/justinhendrickson/Downloads/Cart-Key-Branch-Lookup/apps/waveshare-p4-nano-10-1-nano/tools/lvgl-editor-dashboard
```

On this Mac, validation passes with LVGL Pro CLI v1.2.0. Code generation also passes in a temp copy with `--ignore-images`; full image conversion needs Podman because `golfone_logo_dashboard` is an embedded image asset.

Generated C should be treated as experimental until it is wired behind a firmware flag.
