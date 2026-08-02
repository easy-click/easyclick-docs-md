---
title: Image/color matching
description: Template matching, color-offset matching, and workflow orchestration
sidebar_label: Image/color matching
keywords:
 - image match
 - template match
 - color-offset match
 - matchTemplate
---

# Image/color matching

When the UI has **no stable nodes** (games, custom drawing, icon buttons), use **image/color matching** to locate small images on screen, then **tap** to act.

:::tip vs. node capture

| Method | Best for |
|------|------|
| **Node capture** (`fetch_nodes`) | Standard UI tree, text labels — most reliable |
| **Template match** (`match_template_*`) | Fixed icons, button styles |
| **Color-offset match** (`find_image_by_color_*`) | Transparent background, color-offset small images |
| **OCR** | Text recognition |
| **VLM visual locate** | Natural language element location |

:::

## Four-step pattern in workflows

```
Screenshot (optional) → Match → Match data extraction → Tap
```

1. **Match**: `Non-automation template match` / `Automation template match`, or color-offset equivalents
2. **Match data extraction** (`pick_match`): take **center point** or **highest similarity** match from results
3. **Tap**: `click_point` reads previous step variable

You can skip a separate screenshot step: match steps **auto-capture current screen** when no “reference screenshot variable” is set.

## Template match vs. color-offset match

| Step | Algorithm | Template traits |
|------|------|----------|
| `match_template_no_auto` / `_auto` | OpenCV template match | Normal PNG/JPG icons |
| `find_image_by_color_no_auto` / `_auto` | Color-offset match | Transparent, semi-transparent, large color deviation |

Naming matches screenshot/OCR:

| Suffix | Automation required? |
|------|----------------|
| **Non-automation** (Bridge capture) | **No** |
| **Automation** (Agent capture) | **Yes** (run `Start automation environment` first) |

## Where template images come from

| Method | Description |
|------|------|
| **Local path** | Set `templatePath` relative to flow **working directory**, e.g. `assets/templates/btn_ok.png` |
| **Base64** | Advanced: `templateBase64` embeds small image directly |
| **Editor preview crop** | Trial device online → step card **Match preview** → select region → **Crop image** save as template |

Recommended: under **Flow config → Working directory**, create `assets/templates/` and backup templates with the flow.

## Match preview (editor)

With match or color-offset step selected and trial device chosen:

| Button | Action |
|------|------|
| **Match preview** | Capture + match; show match boxes on canvas |
| **Region select** | Draw **search area** on large image; writes step `region` |
| **Crop image** | Save selected region as template file |
| **Re-match** | Re-run match after screen change |

Color-offset steps have the same preview; parameter area has **color tolerance**, **direction**, etc.

## Main parameters (reference)

### Template match `match_template_*`

| Parameter | Description |
|------|------|
| `saveAs` | Result variable name (required) |
| `templatePath` / `templateBase64` | Small image (one required) |
| `imageFromParam` | Optional; reference previous screenshot variable’s `imageBase64` |
| `region` | Optional; search only within screen rectangle |
| `weakThreshold` / `threshold` | Similarity threshold (default ~0.9) |
| `limit` | Max matches returned |

### Color-offset match `find_image_by_color_*`

| Parameter | Description |
|------|------|
| `saveAs` | Result variable name |
| `templatePath` / `templateBase64` | Color-offset small image |
| `colorOffset` | Color tolerance |
| `direction` | Search direction (0–3) |
| `limit` | Max points returned |

### Match data extraction `pick_match`

| Parameter | Description |
|------|------|
| `fromParam` | Points to earlier match `saveAs` |
| `saveAs` | Output coordinate variable |
| `pick.mode` | `center` (center point), `best` (highest similarity), `find` (by condition + index) |

Match result `x/y` is **template top-left**; for button center use `pick_match` **`center`** mode.

## Typical flow examples

**Fixed icon button:**

1. `match_template_no_auto` → save as `match_1`
2. `pick_match`: `fromParam=match_1`, `mode=center` → `btn_point`
3. `click_point`: `fromParam=btn_point`

**Capture first, match on same image:**

1. `capture_screen_no_auto` → `screen_1`
2. `match_template_no_auto`: `imageFromParam=screen_1`
3. Continue as above

## What to say in AI chat

With LLM configured:

- “On first device do **non-automation template match**, template is xxx.png”
- “**Automation template match** login button, then tap center”

Complex template crop and region limits are easier in **Workflow editor** **Match preview**.

## FAQ

| Symptom | Fix |
|------|------|
| No match found | Lower threshold, narrow `region`, clearer template; for color-offset images try **color-offset match** |
| Automation match fails | **Start automation environment** first; or use **non-automation template match** |
| Tap offset | Use `pick_match` **center**; don’t use match top-left directly |
| Template path not found | Confirm **working directory** + `assets/templates/` path |

More step fields: [Workflow step reference](./workflow-steps#9-imagecolor-recognition).
