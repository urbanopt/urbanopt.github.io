---
layout: default
title: Linux Installation
parent: Installation
nav_order: 3
---

## Linux installation

**_Linux installation/operation has not been tested exhaustively. Please submit an issue if you find something that doesn't work_**

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Manually install [libpng12](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)  
If you get gdbm libs errors like this:

```terminal
/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm.so.3: cannot open shared object file: No such file or directory
```

or this:

```terminal
/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm_compat.so.3: cannot open shared object file: No such file or directory
```

You need to create symlinks to the appropriate version (`libgdbm.so.5` and `libgdbm_compat.so.4`)