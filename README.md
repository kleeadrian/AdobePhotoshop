# Adobe Photoshop 1.0.1 — Source Code

Historical source code for **Adobe Photoshop version 1.0.1** (1990), originally written by Thomas and John Knoll. This release was made available by the [Computer History Museum](https://computerhistory.org/blog/adobe-photoshop-source-code/) with permission from Adobe Systems Inc.

> **Note:** This repository contains **source code only**. It does not include a runnable application for modern macOS, and it cannot be built without additional software that is not part of this release.

## About

Photoshop began in the late 1980s as a program called *Display*, written by Thomas Knoll. Adobe licensed and shipped version 1.0 in early 1990. This archive is version **1.0.1**, a bug-fix release that added PixelPaint import, improved color separation, and fixed TIFF/GIF encoding issues.

| | |
|---|---|
| **Release** | 1990 (version 1.0.1) |
| **Platform** | Classic Mac OS (680x0 Macintosh) |
| **Language** | ~75% Object Pascal, ~15% 680x0 assembly |
| **Framework** | [MacApp](https://en.wikipedia.org/wiki/MacApp) 1.x (not included) |
| **Build system** | MPW (Macintosh Programmer's Workshop) |
| **Application signature** | `8BIM` |
| **Lines of code** | ~128,000 across 179 files |

## License

This material is **(C) Copyright 1990 Adobe Systems Inc.**

It is licensed for **non-commercial use only** under the [Computer History Museum Photoshop License Agreement](https://www.computerhistory.org/softwarelicense/photoshop/). You may not redistribute it to third parties or use it commercially without separate permission.

## What Is Included

- Pascal source (`.p`) and include files (`.inc1.p`, `.p.inc`)
- 680x0 assembly routines (`.a`, `.a.inc`) for performance-critical image operations
- Rez resource definitions (`.r`) for menus, dialogs, icons, and brush tips
- MPW makefiles (`Photoshop.make`, `PaletteWDEF.make`, `MovableWDEF.make`)
- Plugin interface definitions (`FilterInterface.p`, `AcquireInterface.p`, `ExportInterface.p`)
- Change history (`ChangeHistory.txt`)

## What Is NOT Included

The release is **incomplete by design**. Adobe could not redistribute:

- **MacApp 1.x** — Apple's Object Pascal application framework (required to build)
- **EVEITF.o** — Serial-number verification object file (dead code in this build)
- A pre-compiled **Photoshop application**

## Architecture

Photoshop 1.0.1 is built on MacApp's document/view/command pattern:

```
MPhotoshop.p          Program entry point
UPhotoshop.p          Application shell, document model, canvas view
UVMemory.p            Paged virtual memory for large images (TVMArray)
UCommands.p           Undo/redo command framework
UDraw.p               Drawing tools (brush, pencil, eraser, etc.)
USelect.p             Selection tools (marquee, lasso, magic wand)
UAdjust.p             Image adjustments (levels, curves, hue/saturation)
UFilters.p            Built-in filters (blur, sharpen, mosaic, etc.)
UInitFormats.p        File format registration
U*Format.p            Per-format readers/writers (TIFF, GIF, EPS, etc.)
```

Assembly companions (`.a`) exist for the hot paths in drawing, selection, filters, transforms, color conversion, and file I/O. See `Photoshop.make` for the full link list.

## Supported File Formats

| Format | Files |
|--------|-------|
| Native Photoshop (`8BIM`) | `UInternal.p` |
| TIFF | `UTIFFormat.p` |
| GIF | `UGIFFormat.p` |
| EPS / DCS | `UEPSFormat.p` |
| PICT (file & resource) | `UPICTFile.p`, `UPICTResource.p` |
| MacPaint | `UMacPaint.p` |
| Amiga IFF/ILBM | `UIFFFormat.p` |
| Targa (TGA) | `UTarga.p` |
| Raw | `URawFormat.p` |
| PixelPaint | `UPixelPaint.p` |
| Pixar | `UPixar.p` |
| Scitex CT | `UScitexFormat.p` |
| ThunderScan | `UThunderScan.p` |

## Building (Classic Mac Only)

This code targets **Classic Mac OS** and cannot be compiled on modern macOS without a retro development environment.

### Requirements

1. **Classic Mac OS** (System 6 or 7) in an emulator or vintage hardware
2. **MPW 2.x or 3.x** — Macintosh Programmer's Workshop
3. **MacApp 1.x** (~1.1) — Apple's developer framework (not in this repo)
4. **680x0 Mac or emulator** — e.g. [Mini vMac](https://minivmac.sourceforge.net/), [Basilisk II](https://basilisk.legends.net/)

### Build steps

1. Obtain and build MacApp 1.x using its `MABuild` script.
2. Fix source formatting for MPW if needed (strip Windows line endings, convert Rez comments from `{...}` to `//`, restore MPW makefile escape characters).
3. Set MPW search paths to MacApp and this source tree.
4. Run the build defined in `Photoshop.make` via MacApp's `MABuild`.
5. Output is a Mac application named **Photoshop** (creator `8BIM`).

For a detailed account of one successful build, see [Building Photoshop](http://basalgangster.macgui.com/RetroMacComputing/The_Long_View/Entries/2013/3/30_Building_Photoshop.html) by Basalgangster.

## Running on a Modern Mac

This source code **does not run natively** on Apple Silicon or Intel Macs running macOS. Options:

| Goal | Approach |
|------|----------|
| **Use Photoshop 1.0** | Run the original compiled app in a Classic Mac emulator (Mini vMac, Basilisk II) |
| **Study the code** | Read the Pascal and assembly directly — no build required |
| **Build from source** | Set up a Classic Mac + MPW + MacApp environment (see above) |
| **Modern rewrite** | Use this code as an algorithm reference; reimplement in Swift/Metal |

## Project Structure

```
MPhotoshop.p              Main program
UPhotoshop.p              Core application and document model
UVMemory.p                Virtual memory / tile system
UConstants.p              Command IDs, format codes, constants
Photoshop.make            MPW build file
Photoshop.r               Main menu, dialog, and window resources

U*Format.p                File format handlers
U*.a                      680x0 assembly performance routines
*.r                       Rez resource files (About, Tips, Tables, etc.)
*Interface.p              Plugin ABIs (filter, acquire, export)
PaletteWDEF.p             Custom palette window definition
MovableWDEF.p             Custom movable window definition
ChangeHistory.txt         Version changelog
```

## Key Features (circa 1990)

- RGB, CMYK, indexed, grayscale, HSL, and HSB color modes
- Drawing tools: pencil, brush, airbrush, eraser, paint bucket, gradient
- Selection tools: marquee, lasso, magic wand, feather, grow
- Adjustments: levels, curves, brightness/contrast, hue/saturation, invert
- Filters: Gaussian blur, unsharp mask, mosaic, noise, and more
- Transforms: rotate, flip, skew, perspective, stretch/shrink, resample
- Floating selections with blend modes
- Channel operations and alpha channels
- CMYK color separation with halftone screens
- PostScript printing and EPS export
- Filter, scanner, and export plugin support (v3 plugin API)
- Undo/redo via command pattern

## References

- [Computer History Museum — Adobe Photoshop Source Code](https://computerhistory.org/blog/adobe-photoshop-source-code/)
- [CHM Photoshop License Agreement](https://www.computerhistory.org/softwarelicense/photoshop/)
- [1990 Photoshop User Guide (PDF)](https://computerhistory.org/blog/adobe-photoshop-source-code/) — linked from CHM page
- [Building Photoshop (2013)](http://basalgangster.macgui.com/RetroMacComputing/The_Long_View/Entries/2013/3/30_Building_Photoshop.html) — detailed build walkthrough
- Grady Booch's commentary on the source code (included in the CHM blog post)

## Acknowledgments

Created by **Thomas Knoll** and **John Knoll**. Released for non-commercial study by the **Computer History Museum** with permission from **Adobe Systems Inc.**
