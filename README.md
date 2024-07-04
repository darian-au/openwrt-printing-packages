# Printing packages for OpenWrt

This is a [package feed] aiming at providing a complete printing stack
for OpenWrt.

Notably it has:
- Ghostscript 10.03.1
- Gutenprint 5.3.4
- Cups 2.4.10
- OpenPrinting's cups-filters 1.28.17
- Poppler 24.07.0
- QPDF 11.9.1
- Fontconfig 2.15.0
- many other packages to make sure the ones above work...

[package feed]: https://openwrt.org/docs/guide-developer/feeds

### To use this feed,

- set up your router to use [external storage] for its root file
  system, as these packages require more than a 100 MB of space.

[external storage]: https://openwrt.org/docs/guide-user/additional-software/extroot_configuration

- set up a [cross-compilation environment]

[cross-compilation environment]: https://openwrt.org/docs/guide-developer/toolchain/crosscompile

- add this line to your `feeds.conf` or `feeds.conf.default`

```
src-git printing https://github.com/darian-au/openwrt-printing-packages.git
```

- to compile everything in this feed you should use the script `setup-buildsystem.sh` or some variation of those commands.

- copy compiled packages to your router (copy the whole directory as you need the files used to index the packages)

```
scp -r ./bin/$ARCH/packages root@openwrt.lan:/storage/printer/packages/
```

- add local package source to the opkg configuration `/etc/opkg.conf` with

```
src/gz printing file:/storage/printer/packages
```

- see `opkg-install-printing-packages.sh` to see a suggestion of what to install.

- tested against OpenWrt's development trunk.

### Issues / Missing / TODO

Caveat: Ghostscript lacks proper cross-compilation support. I used a
patch taken from [timesys.com] and [termux-packages]. If your architecture is not there,
compiling it just won't work for you.

[termux-packages]: https://github.com/termux/termux-packages/tree/master/packages/ghostscript
[timesys.com]: http://repository.timesys.com/buildsources/g/ghostscript/

The alternative for those who can't compile Ghostscript is to use a
different PDF backend, in this case Poppler. For instructions of how
to do this open the tar-ball of the `cups-filters-*.tar.bz2` and check
the section *1. Selection of the renderer: Ghostscript, Poppler, or
Adobe Reader* of the `README`.
