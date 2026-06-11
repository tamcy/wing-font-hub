# wing-font-hub

CDN-style hosting for the source TTF files used by
[wing-font-generator](https://github.com/chunlaw/wing-font-generator).

This repository exists only to serve font binaries over GitHub Pages
at <https://wing-font-hub.chunlaw.io/>, so the main `wing-font-generator`
repo can stay small and Git-LFS-free. Every file here is a real font
made by someone else — the heavy lifting was done upstream; we just
re-host the binaries so a small project can keep its clones light
and CI bandwidth free.

## How to use it

If you're building Wing Fonts locally, you don't fetch from this repo
manually. Just clone `wing-font-generator` and run:

```sh
python python/init_fonts.py
```

The script reads its list of expected filenames and downloads each
from `https://wing-font-hub.chunlaw.io/<filename>` into
`python/input_fonts/`. It's idempotent — files already present are
left in place.

If you want to test a fork's mirror, set the `WING_FONT_INPUTS_URL`
env var:

```sh
WING_FONT_INPUTS_URL=https://your-user.github.io/your-fork python python/init_fonts.py
```

## Fonts hosted here

Each font keeps its upstream license; this repo doesn't relicense
anything. Click the source link for the canonical release, license
text, and any reserved-font-name terms.

| Filename | Upstream | License |
| --- | --- | --- |
| `ChironSungHK-R.ttf` | [chiron-fonts/chiron-sung-hk](https://github.com/chiron-fonts/chiron-sung-hk) | SIL OFL 1.1 |
| `ChironSungHK-R-It.ttf` | [chiron-fonts/chiron-sung-hk](https://github.com/chiron-fonts/chiron-sung-hk) | SIL OFL 1.1 |
| `ChironHeiHK-R.ttf` | [chiron-fonts/chiron-hei-hk](https://github.com/chiron-fonts/chiron-hei-hk) | SIL OFL 1.1 |
| `ChironHeiHK-B.ttf` | [chiron-fonts/chiron-hei-hk](https://github.com/chiron-fonts/chiron-hei-hk) | SIL OFL 1.1 |
| `Huninn-Regular.ttf` | [justfont/open-huninn-font](https://github.com/justfont/open-huninn-font) (jf-openhuninn 粉圓體) | SIL OFL 1.1 |
| `NotoSerif-Regular.ttf` | [notofonts/latin-greek-cyrillic](https://github.com/notofonts/latin-greek-cyrillic) | SIL OFL 1.1 |
| `NotoSansTC-VariableFont_wght.ttf` | [notofonts/noto-cjk](https://github.com/notofonts/noto-cjk) | SIL OFL 1.1 |
| `NotoSansSC-VariableFont_wght.ttf` | [notofonts/noto-cjk](https://github.com/notofonts/noto-cjk) | SIL OFL 1.1 |
| `NotoSansHK-VariableFont_wght.ttf` | [notofonts/noto-cjk](https://github.com/notofonts/noto-cjk) | SIL OFL 1.1 |
| `NotoSansJP-VariableFont_wght.ttf` | [notofonts/noto-cjk](https://github.com/notofonts/noto-cjk) | SIL OFL 1.1 |
| `NotoSansKR-VariableFont_wght.ttf` | [notofonts/noto-cjk](https://github.com/notofonts/noto-cjk) | SIL OFL 1.1 |
| `SourceHanSerif-Regular.ttf` | [adobe-fonts/source-han-serif](https://github.com/adobe-fonts/source-han-serif) | SIL OFL 1.1 |
| `XiaolaiSC-Regular.ttf` | [lxgw/Lxgw-Wenkai-Screen / Xiaolai](https://github.com/lxgw/Lxgw-Wenkai-Screen) | SIL OFL 1.1 |
| `mplus-1m-medium.ttf` | [coz-m/mplus_outline_fonts](https://github.com/coz-m/mplus_outline_fonts) | SIL OFL 1.1 |
| `MPLUSRounded1c-Regular.ttf` | [coz-m/mplus_outline_fonts](https://github.com/coz-m/mplus_outline_fonts) | SIL OFL 1.1 |
| `GoogleSans-VariableFont_GRAD,opsz,wght.ttf` | [itfoundry/google-sans-thai](https://github.com/itfoundry/google-sans-thai) | SIL OFL 1.1 |

The `LICENSES/` folder contains the canonical OFL-1.1 text plus a
per-font credits file that lists each font's copyright statement and
any reserved-font-name conditions. If you redistribute any of these
files — including via a fork of this repo — you must keep those
files alongside the binaries (OFL Section 4).

## How this repo is used by wing-font-generator's CI

The build pipeline in
[`wing-font-generator/.github/workflows/deploy-pages.yml`](https://github.com/chunlaw/wing-font-generator/blob/main/.github/workflows/deploy-pages.yml)
runs `python init_fonts.py` once per matrix slot (with an
`actions/cache` layer keyed on `init_fonts.py`'s contents, so the
download only happens when the font list changes). At steady state a
font-touching commit pulls one fresh copy of each TTF (~250 MB across
all matrix slots combined) and then caches them for subsequent runs.
GitHub Pages' bandwidth quota is "soft-capped" at 100 GB/month per
site, which we're not at risk of approaching.

## Forking this repo

If you want to add a font that Wing Font's pipeline doesn't yet
ship, the path is:

1. Fork this repo.
2. Drop the new TTF at the root of your fork's served branch. Make
   sure the file is OFL/Apache/SIL-licensed and that you include the
   upstream license text in `LICENSES/`.
3. Add the filename to `FONT_FILES` in `wing-font-generator/python/init_fonts.py`.
4. If your fork uses a custom domain, update `DEFAULT_BASE_URL` in
   the same file, or set `WING_FONT_INPUTS_URL` at runtime.
5. Open a PR against `wing-font-generator` with the `init_fonts.py`
   change. If it's useful for everyone, we'll also pull the binary
   into this canonical repo.

## License (this repository's contents)

The font binaries are each licensed by their respective upstreams
(all SIL OFL 1.1, see the table above). The README and any other
metadata files added by this hosting layer are released into the
public domain under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) —
i.e. you can reuse the README structure, the `init_fonts.py`-
compatible filename convention, or anything else in this repo
that isn't itself a font, without any obligation.
