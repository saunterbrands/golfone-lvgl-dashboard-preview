# Firmware Integration Notes

Use exported C from this project, not runtime XML loading, for the ESP32-P4 firmware.

Reason:

- The current firmware is on LVGL 9.5.0 with RGB565 assets and no XML runtime enabled in `sdkconfig`.
- LVGL Editor C export does not require `LV_USE_XML`.
- The existing firmware already owns the live Blender map/background and zoom/pan source pixels. The exported UI should provide live overlay elements, not a second stale background pipeline.

Recommended staged integration:

1. Export C from `tools/lvgl-editor-dashboard`.
2. Add the generated source tree to the ESP-IDF build behind an experimental option such as `CONFIG_DASHBOARD_EDITOR_UI_EXPERIMENTAL`.
3. Mount only `left_footer_create(lv_obj_t * parent, lv_subject_t * pace_subject, lv_subject_t * clock_subject)` first, as a child of `g_map_overlay`.
4. Update `dashboard_clock_label` from `formatDashboardClockLabel(...)` inside `onDashboardPanelTimer`, using the generated string subject.
5. Keep `pin_readout_create(lv_obj_t * parent, lv_subject_t * yards_subject, lv_subject_t * unit_subject, lv_subject_t * elevation_subject)` as a separate object with its own invalidation and positioning.
6. Do not replace the current target-mode left overview card until that panel is ported intentionally. It is currently the correct integrated background card.

The current hand-drawn functions in `main/main.cpp` remain the production path until a generated component renders cleanly on hardware:

- `drawDashboardLeftStatusOverlay`
- `drawDashboardRightHoleHeaderOverlay`
- `drawDashboardScorecardOverlay`
- `drawDashboardLiveTipsOverlay`
- `drawDashboardUtilityNavOverlay`
- `onDashboardPinReadoutDraw`
