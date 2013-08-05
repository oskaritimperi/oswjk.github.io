---
layout: post
title: Creating symlinks on Windows XP
---

# {{ page.title }}

Directions can be found [here](http://schinagl.priv.at/nt/hardlinkshellext/hardlinkshellext.html#symboliclinksforwindowsxp).

After installing, you can create symbolic links to network shares
and so on with the tool `ln.exe`:

    C:\symlink> ln.exe -s "\\remote\share" share_from_remote
