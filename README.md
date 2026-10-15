# G'MIC macOS X11 / XQuartz patch

Patch for G'MIC 4.0.6 that enables the X11 display backend on macOS.

## Problem

Interactive G'MIC games and demos from **Games & Demos** can report:

    Command 'x_2048': No display available.

On macOS, the upstream G'MIC-Qt CMake configuration disables CImg display support with:

    -Dcimg_display=0

This prevents interactive G'MIC games and demos from opening their own windows, even when XQuartz is installed and `$DISPLAY` is available.

## Solution

This patch:

- enables X11 detection on macOS;
- builds G'MIC-Qt with `cimg_display=1`;
- links the X11 libraries;
- keeps the existing Android and Windows display handling unchanged.

This restores the X11 display backend required by interactive G'MIC games and demos on macOS, including `x_2048` and other similar commands.

## Requirements

- macOS
- XQuartz
- G'MIC 4.0.6 source
- CMake
- X11 development libraries, for example via Homebrew

Verify X11 with:

    pkg-config --exists x11 && echo "X11 OK"

Verify XQuartz with:

    echo "$DISPLAY"

## Apply

From the root of a clean G'MIC 4.0.6 source tree:

    git apply gmic-4.0.6-macos-xquartz-x11.patch

Then configure and build G'MIC-Qt normally.

## Tested

The patch was tested on Apple Silicon macOS with XQuartz.

With the patched G'MIC-Qt build, interactive G'MIC games and demos can use the X11 display backend successfully.
