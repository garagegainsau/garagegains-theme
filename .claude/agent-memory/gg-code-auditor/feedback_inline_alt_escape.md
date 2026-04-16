---
name: image_tag alt parameter — pre-assignment required
description: The image.alt | escape inline pattern inside image_tag named params caused upload failures; pre-assign to a variable instead
type: feedback
---

Never pass `image.alt | escape` inline as a named parameter to `image_tag`. Pre-assign the escaped value to a variable before the tag call.

**Why:** Caused theme upload failures in a prior version of the codebase. The Liquid parser chokes on filter chains inside named parameter values.

**How to apply:** Always check `image_tag` calls for inline `| escape` on `alt`. The correct pattern is:
```liquid
{%- assign logo_alt = shop.name | escape -%}
{{ image | image_url: width: 280, format: 'webp' | image_tag: alt: logo_alt }}
```
