# Design changes (background tile + content panels)

Date: 2026-11-21

## What changed

1. **Background tile image**
   - Source copied from: `/home/ivan/Pictures/personal_stuff/Prairie_Wind.bmp`
   - Repo copies:
     - `static/img/Prairie_Wind.bmp` (original)
     - `static/img/Prairie_Wind.png` (web-friendly version used by CSS)

2. **Custom CSS**
   - `static/css/site-custom.css`
   - Light mode background: `static/img/tile_light_pw.png`
   - Dark mode background: `static/img/tile_dark_pw.png` (when `html.dark`)
   - Rectangular panels (no rounded corners) at ~78% opacity for readable text
   - Header/menu panel capped at theme container width (`max-width: 50rem`) with horizontal padding so it aligns with content blocks
   - Extra header `padding-right` so Projects + theme toggle aren't flush to the edge
   - Footer: remove theme `h-10` clip so Blog / Designed with / © sit inside the footer panel
   - Light tile brightened toward white (`tile_light_pw.png`; original saved as `tile_light_pw.original.png`)
   - Avoids `prefers-color-scheme` panel overrides (those caused dark panels + black text)

3. **Layout hook**
   - `layouts/_default/baseof.html` (site override of the Astatine base layout)
   - Only addition vs theme: loads `/css/site-custom.css`

4. **Nav fix**
   - `layouts/partials/nav.html` uses `relURL` for theme icons (fixes broken `example.org` icon URLs)

5. **Theme CSS load fix (critical)**
   - `layouts/partials/head.html` uses `$style.RelPermalink` instead of `.Permalink`
   - Prevents loading CSS from `https://example.org/...` on localhost (which made the site look like unstyled HTML)
   - `config.toml` `baseURL` set to `https://idoparin.github.io/`

## How to preview

```bash
cd /home/ivan/proj/homepage
hugo server
```

Open http://localhost:1313/ and hard-refresh if needed (Ctrl+Shift+R).

## How to cancel / revert

Remove the design files and restore the default theme base layout:

```bash
cd /home/ivan/proj/homepage
rm -f static/img/Prairie_Wind.bmp static/img/Prairie_Wind.png
rm -f static/css/site-custom.css
rm -f layouts/_default/baseof.html
rm -f DESIGN_CHANGES.md
```

If already committed:

```bash
git checkout -- static/img/Prairie_Wind.bmp static/img/Prairie_Wind.png \
  static/css/site-custom.css layouts/_default/baseof.html DESIGN_CHANGES.md
# or remove them in a new commit
```

No other content files were modified for this design experiment.
