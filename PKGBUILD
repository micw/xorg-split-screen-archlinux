# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=xorg-server
pkgname=('xorg-server' 'xorg-server-xephyr' 'xorg-server-xdmx' 'xorg-server-xvfb' 'xorg-server-xnest' 'xorg-server-common' 'xorg-server-devel')
pkgver=1.10.3
pkgrel=1
arch=('i686' 'x86_64')
license=('custom')
url="http://xorg.freedesktop.org"
makedepends=('pixman' 'libx11' 'mesa' 'libgl' 'xf86driproto' 'xcmiscproto' 'xtrans' 'bigreqsproto' 'randrproto' 'inputproto' 'fontsproto' 'videoproto' 'compositeproto' 'recordproto' 'scrnsaverproto' 'resourceproto' 'xineramaproto' 'libxkbfile' 'libxfont' 'renderproto' 'libpciaccess' 'libxv' 'xf86dgaproto' 'libxmu' 'libxrender' 'libxi' 'dmxproto' 'libxaw' 'libdmx' 'libxtst' 'libxres' 'xorg-xkbcomp' 'xorg-util-macros' 'xorg-font-util')
options=('!libtool')
source=(${url}/releases/individual/xserver/${pkgbase}-${pkgver}.tar.bz2
        #git-fixes.patch
        bg-none-revert.patch
        xserver-1.10-pointer-barriers.patch
        xorg-redhat-die-ugly-pattern-die-die-die.patch
        autoconfig-nvidia.patch
        xvfb-run
        xvfb-run.1
        10-quirks.conf)
sha1sums=('1699be5c0edeca553cfa3ee6caa228483465136b'
         # '6dd2bcd9d8b17d1a50ed8c15eb1cba480558e695'
          '629c6d8d52126eab81ee1b72a9e4209535f8cb81'
          '1b95e91384a57d966428c7db98ed06f4cc562f91'
          '0efcdf61bde3c0cd813072b94e2b30ab922775b9'
          'f9328fd7bc931bb02c8909ecfcef35403de33782'
          'c94f742d3f9cabf958ae58e4015d9dd185aabedc'
          '6838fc00ef4618c924a77e0fb03c05346080908a'
          '993798f3d22ad672d769dae5f48d1fa068d5578f')

build() {
  cd "${srcdir}/${pkgbase}-${pkgver}"
  # Get rid of the ugly pattern
  patch -Np3 -i "${srcdir}/xorg-redhat-die-ugly-pattern-die-die-die.patch"

  # Add pointer barrier support, patch from Fedora
  patch -Np1 -i "${srcdir}/xserver-1.10-pointer-barriers.patch"

  # Patches from ~ajax/xserver xserver-next branch
  patch -Np1 -i "${srcdir}/bg-none-revert.patch"

  # Upstream fixes from 1.10 branch
  #patch -Np1 -i "${srcdir}/git-fixes.patch"

  # Use nouveau/nv/nvidia drivers for nvidia devices
  patch -Np1 -i "${srcdir}/autoconfig-nvidia.patch"

  autoreconf
  ./configure --prefix=/usr \
      --enable-ipv6 \
      --enable-dri \
      --enable-dmx \
      --enable-xvfb \
      --enable-xnest \
      --enable-composite \
      --enable-xcsecurity \
      --enable-xorg \
      --enable-xephyr \
      --enable-glx-tls \
      --enable-kdrive \
      --enable-install-setuid \
      --enable-config-udev \
      --disable-config-dbus \
      --enable-record \
      --disable-xfbdev \
      --disable-xfake \
      --disable-static \
      --sysconfdir=/etc/X11 \
      --localstatedir=/var \
      --with-xkb-path=/usr/share/X11/xkb \
      --with-xkb-output=/var/lib/xkb \
      --with-fontrootdir=/usr/share/fonts
  make

  sed -e 's/^DMX_SUBDIRS =.*/DMX_SUBDIRS =/' \
      -e 's/^XVFB_SUBDIRS =.*/XVFB_SUBDIRS =/' \
      -e 's/^XNEST_SUBDIRS =.*/XNEST_SUBDIRS = /' \
      -e 's/^KDRIVE_SUBDIRS =.*/KDRIVE_SUBDIRS =/' \
      -i hw/Makefile
}

package_xorg-server-common() {
  pkgdesc="Xorg server common files"
  depends=('xkeyboard-config' 'xorg-xkbcomp' 'xorg-setxkbmap' 'xorg-fonts-misc')

  cd "${srcdir}/${pkgbase}-${pkgver}"
  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-common"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/xorg-server-common"
  
  make -C xkb DESTDIR="${pkgdir}" install-data

  install -m755 -d "${pkgdir}/usr/share/man/man1"
  install -m644 doc/man/Xserver.1 "${pkgdir}/usr/share/man/man1/"

  install -m755 -d "${pkgdir}/usr/lib/xorg"
  install -m644 dix/protocol.txt "${pkgdir}/usr/lib/xorg/"
}

package_xorg-server() {
  pkgdesc="Xorg X server"
  depends=(libxdmcp libxfont udev libpciaccess libdrm pixman libgcrypt libxau xorg-server-common xf86-input-evdev)
  backup=('etc/X11/xorg.conf.d/10-evdev.conf' 'etc/X11/xorg.conf.d/10-quirks.conf')
  provides=('x-server')
  groups=('xorg')

  cd "${srcdir}/${pkgbase}-${pkgver}"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/etc/X11"
  mv "${pkgdir}/usr/share/X11/xorg.conf.d" "${pkgdir}/etc/X11/"
  install -m644 "${srcdir}/10-quirks.conf" "${pkgdir}/etc/X11/xorg.conf.d/"

  rmdir "${pkgdir}/usr/share/X11"

  # Needed for non-mesa drivers, libgl will restore it
  mv "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so" \
     "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.xorg"

  rm -rf "${pkgdir}/var"

  rm -f "${pkgdir}/usr/share/man/man1/Xserver.1"
  rm -f "${pkgdir}/usr/lib/xorg/protocol.txt"

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server/COPYING"

  rm -rf "${pkgdir}/usr/lib/pkgconfig"
  rm -rf "${pkgdir}/usr/include"
  rm -rf "${pkgdir}/usr/share/aclocal"
}

package_xorg-server-xephyr() {
  pkgdesc="A nested X server that runs as an X application"
  depends=(libxfont libgl libgcrypt libxv pixman xorg-server-common)

  cd "${srcdir}/${pkgbase}-${pkgver}/hw/kdrive"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xephyr"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xephyr/COPYING"
}

package_xorg-server-xvfb() {
  pkgdesc="Virtual framebuffer X server"
  depends=(libxfont libxdmcp libxau libgcrypt pixman xorg-server-common)

  cd "${srcdir}/${pkgbase}-${pkgver}/hw/vfb"
  make DESTDIR="${pkgdir}" install

  install -m755 "${srcdir}/xvfb-run" "${pkgdir}/usr/bin/"
  install -m644 "${srcdir}/xvfb-run.1" "${pkgdir}/usr/share/man/man1/"

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xvfb"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xvfb/COPYING"
}

package_xorg-server-xnest() {
  pkgdesc="A nested X server that runs as an X application"
  depends=(libxfont libxext libgcrypt pixman xorg-server-common)

  cd "${srcdir}/${pkgbase}-${pkgver}/hw/xnest"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xnest"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xnest/COPYING"
}

package_xorg-server-xdmx() {
  pkgdesc="Distributed Multihead X Server and utilities"
  depends=(libxfont libxi libgcrypt libxaw libxrender libdmx libxfixes pixman xorg-server-common)

  cd "${srcdir}/${pkgbase}-${pkgver}/hw/dmx"
  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-xdmx"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-xdmx/COPYING"
}

package_xorg-server-devel() {
  pkgdesc="Development files for the X.Org X server"
  depends=(xproto randrproto renderproto xextproto inputproto kbproto fontsproto videoproto dri2proto xineramaproto xorg-util-macros pixman libpciaccess)

  cd "${srcdir}/${pkgbase}-${pkgver}"
  make DESTDIR="${pkgdir}" install

  rm -rf "${pkgdir}/usr/bin"
  rm -rf "${pkgdir}/usr/share/man"
  rm -rf "${pkgdir}/usr/share/doc"
  rm -rf "${pkgdir}/usr/share/X11"
  rm -rf "${pkgdir}/usr/lib/xorg"
  rm -rf "${pkgdir}/var"

  install -m755 -d "${pkgdir}/usr/share/licenses/xorg-server-devel"
  ln -sf ../xorg-server-common/COPYING "${pkgdir}/usr/share/licenses/xorg-server-devel/COPYING"
}
