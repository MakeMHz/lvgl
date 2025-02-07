# Roadmap

## Planned for v9

### Naming and API
- [x] `lv_style_set_size()` should have separate width and height parameters
- [x] Reconsider image color formats.
- [ ] More consistent names:`remove/clear/delete/del`, `offset/ofs`, `add/create/register`, `id/idx/index`, `middle/mid/center`, `img/image`, `txt/text`, `opa/opacity/alpha`, `scr/screen`, `disp, display`, `finished/complete/completed/ready`, `buf/buffer`, `..._cb`, angle, rotation`, `zoom, scale`
- [ ] Reconsider the use of `has`, `is`, `enable` "action" keywords
- [x] Update canvas API (https://github.com/lvgl/lvgl/issues/3393)
- [x] `LV_STYLE_PROP_INHERIT` -> `LV_STYLE_PROP_FLAG_INHERITABLE` [LINK](https://github.com/lvgl/lvgl/pull/3390#discussion_r885915769)
- [x] Replace `disp_drv->direct_mode/full_refresh` with enum.
- [x] Consider flat directory structure. E.g. `extra/widgets` to `widgets`
- [ ] Use `uint32_t` and `int32_t` in APIs where possible. Consider hardcoding `lv_coord_t` as `int32_t`.
- [ ] To define a new stdlib API use defines `LV_USE_CUSTOM_...` and let the user implement `lv_...` functions somewhere (instead of defining the name of the custom functions)
- [ ] Generic `lv_date_t` and `lv_time_t`
- [ ] Make layouts with an `lv_layout_dsc_t` instead of registering an ID + callback. See [here](https://github.com/lvgl/lvgl/issues/3481#issuecomment-1206434501)

### Architecture
- [x] Consider merging `lv_disp_drv_t`, `lv_disp_t`, `lv_disp_draw_buf_t`, `lv_draw_ctx_t`, and `struct`s from the new driver API (or only some of them)
- [x] New driver architecture #2720
- [x] `draw_ctx->buffer_convert`? See [here](https://github.com/lvgl/lvgl/issues/3379#issuecomment-1147954592).  Also remove 16 SWAPPED color format? See [here](https://github.com/lvgl/lvgl/issues/3379#issuecomment-1140886258).
- [ ] `lv_img_set_src()` use "type-aware" parameter and rework image decoders. See [here](https://github.com/lvgl/lvgl/tree/arch/img-decode-rework)
- [ ] Reconsider masks. There should be a generic high level mask API which is independent of the drawing engine. [#4059](https://github.com/lvgl/lvgl/issues/4059)
- [x] `get_glyph_bitmap` should return an a8 bitmap that can be blended immediately.
- [ ] Reconsider how themes should work. See [here](https://github.com/lvgl/lvgl/pull/3390#pullrequestreview-990710921).
- [ ] Make LVGL render independent areas in parallel. [#4016](https://github.com/lvgl/lvgl/issues/4016)
- [x] Drop `lv_mem_buf_get` as tlsf should be fast enough for normal allocations too. Fragmentation is also lower if processes can completely clean up after themselves.
- [x] More color formats: 24 bit, ARGB1555, ARGB4444 etc
- [ ] Unified caching #3116 #3415
- [ ] Condider using `lv_color32_t` on APIs to support e.g. alpha gradient.

### Styles
- [ ] Make `style_bg_img` support `9patch` images
- [ ] non-uniform scale of images: scale width and height differently
- [ ] Scroll anim settings should come from styles to allow customization

### Widgets
- [ ] `lv_img`: Reconsider image sizing models (when the image size is not content): center, top-left, zoom, tile, other?
- [ ] `lv_tabview` Replace button matrix with real buttons for more flexibility
- [ ] `lv_label` reconsider label long modes. (support min/max-width/height too) #3420
- [ ] `lv_roller` make it more flexible #4009
- [ ] Universal scale widget/support (see [here](https://forum.lvgl.io/t/linear-meter-bar-with-ticks/10986) and #4043)
- [ ] Improve `lv_label_align_t` #1656
- [ ] Disabled widgets should absorb indev actions without sending events. [#3860](https://github.com/lvgl/lvgl/issues/3860)

### Drawing and rendering
- [ ] Automatically recalculate the layout if a coordinate is get with `lv_obj_get_width/height/x/y/etc`

 ### Animations
- [ ] Use `anim` events to replace many callbacks with one
- [ ] `lv_anim_time_to_speed` should work differently to remove `style_anim_speed`. E.g. on large values of anim time store the speed.   Besides all widgets should use the `style_anim` property. `anim` should clamp the time if it's calculated from speed, e.g `lv_clamp(200, t, 2000)`. (maybe `a->min_time/max_time`).
- [ ] Use dedicated `lv_anim_custom_exec_cb_t`. See [here](https://forum.lvgl.io/t/custom-exec-cb-prevents-lv-anim-del-obj-null/10266).

## Planned in general

### CI
- [ ] Platform independent benchmarking #3443
- [ ] Run static analyzer
- [ ] Release script
- [ ] Unit test for all widgets #2337
- [ ] CI test for flash/RAM usage #3127

### Architecture
- [ ] C++ binding: https://github.com/lvgl/lv_binding_cpp
- [ ] Markup language #2428

### Styles
- [ ] Hover
- [ ] Global states in selectors. E.g. `LV_STATE_PRESSED | SMALL_SCREEN` like media quarry in CSS

### Drawing and rendering
- [ ] SW: Line drawing with image rotation
- [ ] SW: Arc drawing from small squares (16x16?) to detect transparent or solid parts
- [ ] SW: Rounded rectangle drawing from small squares (16x16?) to detect transparent or solid parts
- [ ] Different radius on each corner #2800
- [ ] Gradient to border/outline/shadow
- [ ] Multiple shadow/border
- [ ] Perspective
- [ ] Text shadow
- [ ] Innter shadow
- [ ] ARGB image stroke/grow on the alpha map
- [ ] Real time blur
- [ ] Gradient with alpha

### Widgets
- [ ] `lv_bar`, `lv_arc`: handle max < min for fill direction swapping #4039 

### Others
- [ ] More grid features. E.g. repeat(auto-fill, minmax( <value> px, 1fr))
- [ ] Named grid cells to allow updating layouts without touching the children (like CSS `grid-template-areas`)
- [ ] Scene support. See [this comment](https://github.com/lvgl/lvgl/issues/2790#issuecomment-965100911)
- [ ] Circle layout. #2871
- [ ] Variable binding. I.e create properties which can be bound to objects and those objects are notified on value change. Maybe based on `lv_msg`?
- [ ] Consider [stagger animations](https://greensock.com/docs/v3/Staggers).
 
## Ideas
- Better way to reset global variables in `lv_deinit()` #3385
- `lv_array`: replace linked lists with array where possible (arrays are faster and uses less memory)
- Reconsider how to handle UTF-8 characters (allow different encoding too) and Bidi. Maybe create an abstraction for typesetting.
- Consider direct binary font format support
- Improve groups. [Discussion](https://forum.lvgl.io/t/lv-group-tabindex/2927/3). Reconsider focusing logic. Allow having no widget selected (on web it's possible). Keep editing state in `lv_obj_t` (See [here](https://github.com/lvgl/lvgl/issues/3646)). Support slider left knob focusing (see [here](https://github.com/lvgl/lvgl/issues/3246))
- lv_mem_alloc_aligned(size, align)
- Speed up font decompression
- Support larger images: add support for large image #1892
- Functional programming support, pure view? See [here](https://www.freecodecamp.org/news/the-revolution-of-pure-views-aed339db7da4/)
- Style components. See [this comment](https://github.com/lvgl/lvgl/issues/2790#issuecomment-965100911)
- SVG support: integrate an SVG render library
- Support dot_begin and dot_middle long modes for labels
- Allow matrix input for image transformation?
- Radial/skew/conic gradient
- Somehow let children inherit the parent's state
- Text on path
