# Quake2 Port GPL

This is a patched (bugfixed, not improved) copy of the Quake2 tree based on the
GPL 3.21 sources as published to idstuff.

The goal of this fork is to maintain an authentic port for various platforms
(mostly so we can show off unusual platforms from the late 90s/early 00s at
vintage computing events), not an improved one.  If you want an improved port, 
you probably want to look at Quake2RTX, q2vkpt, or some of the other Quake2 
ports.

Because the main goal is targeting non-x86 systems, the inline assembler has 
been inhibited and will eventually be removed making this a pure C port only.

If you just want to play Quake2 on Windows, you should probably buy the version
of the game on Steam which incorporates significant meaningful modernisations
as well as including the original build and data.

## Changes

Refer to the commit logs, but a general list:

* Disabled assembly code segments, using C-only paths
* Switched to cmake for most build targets
* Fixed x64 Windows crashes (file system traversal API issues)
* Fixed bug in ref_soft causing 64-bit targets to glitch with transparent surfaces.

## Building

Use cmake to build and install.   Binaries will be installed into the
`CMAKE_INSTALL_PREFIX` assuming that it is your Quake2 tree (game DLLs/sos will
be placed accordingly)

## Target Status

| OS      | Arch | Status   |
| ------- | ---- | -------- |
| Windows | x86  | Working  |
| Windows | x64  | Working  |

## From the original source readme

The code is all licensed under the terms of the GPL (gnu public license).  
You should read the entire license, but the gist of it is that you can do
anything you want with the code, including sell your new version.  The catch
is that if you distribute new binary versions, you are required to make the
entire source code available for free to everyone.

The primary intent of this release is for entertainment and educational
purposes, but the GPL does allow commercial exploitation if you obey the
full license.  If you want to do something commercial and you just can't bear
to have your source changes released, we could still negotiate a separate
license agreement (for $$$), but I would encourage you to just live with the
GPL.

All of the Q2 data files remain copyrighted and licensed under the
original terms, so you cannot redistribute data from the original game, but if
you do a true total conversion, you can create a standalone game based on
this code.
