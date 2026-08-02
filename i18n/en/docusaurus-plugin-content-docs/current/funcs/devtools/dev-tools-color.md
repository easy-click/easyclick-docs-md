---
title: EasyClick Android Docs — Mobile Automation Scripts — Node and Color Capture
hide_title: false
hide_table_of_contents: false
sidebar_label: Color Tool Panel
description: 'EasyClick mobile automation scripts — configure EasyClick node capture, phone screenshots, and color generation in IDEA'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - EasyClick script node capture
 - color generation
 - png
 - img
 - src
 - androidimg
 - color
 - preview
 - alt
 - style
 - zoom
 - findcolor
 - mobile automation
 - test automation
 - script development
---

# Node and Color Capture
 - Location: `IDEA menu bar - EasyClick Android - Color Tool`
 - <img src='/androidimg/color/preview.png' alt="preview.png" style={{zoom:'30%'}} />

## Capture Image
 - Click the `Capture Image` button to fetch the current phone screen. Check `EasyClick Android Run Log`
 - The `file list` on the left supports file search; the right-click menu can `delete files`, `rename`, and more


## Pick Color
 - Move the mouse over the `image` to see the magnified `color preview`; press the `Space` key to add the current color to the `Find Color` parameter table on the right
 - Use the mouse wheel to `zoom in` or `zoom out` the image
## Pick Image Region
 - Move the mouse over the `image`, hold `Ctrl`, and drag with the left mouse button to capture the selected region into the `Find Image` parameter table on the right
## Region
 - Press and hold the right mouse button and drag on the image to fill the region values in the color tool panel
## Find Color Parameter Panel
 - <img src='/androidimg/color/findcolor.png' alt="findcolor.png" style={{zoom:'30%'}} />
 - The top section shows captured colors, offset colors, and related values
 - The second row configures parameters for generated code; see the image/color function chapter for details
 - The bottom buttons test color parameters and show results
 - `Code Template` is a custom template; clicking `Generate Code` fills parameters from the selected template
## Find Image Parameter Panel
 - <img src='/androidimg/color/findimage.png' alt="findimage.png" style={{zoom:'30%'}} />
 - Hover over the thumbnail and use the wheel to zoom in or out
 - `Convert Find Image to Find Color` randomly samples feature colors from the current image and fills the `Find Color` panel
 - `Color Fill` includes both `crop` and `color fill` modes
 - `Transparent Find Image` works with `color fill`: the four corners are painted the same color; other pixels matching that color are ignored during matching and excluded from color search
 - One-click color fill and precise color fill both help ignore colors you do not want computed
 - Used well, this helps locate image coordinates accurately
 - Options correspond to find-image module functions; read the docs for details
 - `Code Template` is a custom template; clicking `Generate Code` fills parameters from the selected template

## Other Features
 - `Single Tab Mode`: Controls whether images open in a new window or replace the current tab
 - `ADB + Screen Record Mode`: Capture via ADB first; fall back to app capture if ADB fails
 - `Screen Record Only Mode`: Capture using the app only
