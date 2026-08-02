---
title: Native UI Controls
description: EasyClick automation scripts — Android no root — native UI controls
keywords:
 - EasyClick automation scripts Android no root native UI controls
 - en
 - funcs
 - ui
 - uidetail
 - md
 - UI
 - scrollview
 - LinearLayout
 - linearlayout
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
---


## Supported Views
- [LinearLayout](/docs/funcs/ui/uidetail/linearlayout)
- [FrameLayout](/docs/funcs/ui/uidetail/framelayout)
- [RelativeLayout](/docs/funcs/ui/uidetail/relativelayout)
- [ScrollView](/docs/funcs/ui/uidetail/scrollview)
- [HorizontalScrollView](/docs/funcs/ui/uidetail/h_scrollview)
- [View](/docs/funcs/ui/uidetail/view)
- [Button](/docs/funcs/ui/uidetail/button)
- [TextView](/docs/funcs/ui/uidetail/textview)
- [EditText](/docs/funcs/ui/uidetail/edittext)
- [CheckBox](/docs/funcs/ui/uidetail/checkbox)
- [RadioGroup](/docs/funcs/ui/uidetail/radiogroup)
- [RadioButton](/docs/funcs/ui/uidetail/radiobutton)
- [Spinner](/docs/funcs/ui/uidetail/spinner)
- [WebView](/docs/funcs/ui/uidetail/webview)
- [ImageView](/docs/funcs/ui/uidetail/imageview)
- [CardView](/docs/funcs/ui/uidetail/cardview)
- [Switch](/docs/funcs/ui/uidetail/switch)
- [FlexboxLayout](/docs/funcs/ui/uidetail/flexboxlayout)
- [include tag](/docs/funcs/ui/uidetail/includetag)
- [Canvas](/docs/funcs/ui/uidetail/canvas)

## Common Properties

| Property | Description | Values |
| :------: | :------: | :------: |
| layout_width | Width | wrap_content: match content size<br/> match_parent: match parent size<br/>specific number + dp |
| layout_height | Height | wrap_content: match content size<br/> match_parent: match parent size<br/>specific number + dp |
| background | Background color | Hex color, e.g. #FFFFFF or #FFFFFFFF |
| tag | Tag | Chinese or English; retrieve the corresponding value by tag in code |
| visibility | Visibility | gone: hidden<br/>visible: shown<br/>invisible: hidden but occupies space |
| clickable | Clickable | true: clickable <br/>false: not clickable |
| enable | Enabled | true: enabled <br/>false: disabled |
| minHeight | Minimum height | specific number + dp |
| minWidth | Minimum width | specific number + dp |
| paddingLeft | Left padding | specific number + dp |
| paddingTop | Top padding | specific number + dp |
| paddingRight | Right padding | specific number + dp |
| paddingBottom | Bottom padding | specific number + dp |
| padding | Padding on all sides | specific number + dp |
| layout_gravity | Alignment within parent container |[Usage reference](https://blog.csdn.net/gaojinshan/article/details/44917205)<br/>top<br/>bottom<br/>left<br/>right<br/>center_vertical<br/>fill_vertical<br/>center_horizontal<br/>fill_horizontal<br/>center<br/>fill<br/>clip_vertical<br/>clip_horizontal<br/> |
| layout_margin | Margin on all sides | specific number + dp |
| layout_marginLeft | Left margin | specific number + dp |
| layout_marginRight | Right margin | specific number + dp |
| layout_marginTop | Top margin | specific number + dp |
| layout_marginBottom | Bottom margin | specific number + dp |
| cornerRadius | View corner radius | specific number + dp |


## UI Height and Width
- Properties: `layout_width`, `layout_height`
- `match_parent` — fill the parent container
- `wrap_content` — size based on actual content width
- Numeric values, e.g. `12dp`, represent 12 dp units (`dp` is Android's density-independent pixel unit)

## Reading UI Parameters

- See [Global Module — UI Parameter Reading](/docs/funcs/global#ui参数读取)
