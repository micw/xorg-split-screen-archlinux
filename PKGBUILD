# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=xorg-server
pkgver=1.7.2
pkgrel=2
pkgdesc="X.Org X servers"
arch=('i686' 'x86_64')
license=('custom')
url="http://xorg.freedesktop.org"
depends=('hal>=0.5.13' 'libgl' 'libxfont>=1.4.1' 'openssl>=0.9.8k' 'libpciaccess>=0.10.9' 'libxv>=1.0.5' 'pixman>=0.16.2' 'xcursor-themes>=1.0.2' 'xkeyboard-config>=1.6' 'xorg-server-utils' 'xorg-fonts-misc' 'xbitmaps' 'diffutils' 'xf86-input-evdev>=2.2.5' 'inputproto>=2.0-1')
makedepends=('libx11>=1.3' 'mesa>=7.6' 'xf86driproto>=2.1.0' 'xtrans>=1.2.4' 'libxkbfile>=1.0.6' 'randrproto>=1.3.1' 'renderproto>=0.11' 'xcmiscproto>=1.2.0' 'bigreqsproto>=1.1.0' 'resourceproto>=1.1.0' 'videoproto>=2.3.0' 'compositeproto>=0.4.1' 'scrnsaverproto>=1.2.0' 'xf86dgaproto>=2.1' 'libgl>=7.6' 'glproto>=1.4.10' 'xorg-util-macros>=1.3.0' 'xineramaproto>=1.2')
conflicts=('catalyst-utils<=9.2' 'xf86-input-calcomp' 'xf86-input-citron' 'xf86-input-digitaledge' 'xf86-input-dmc' 'xf86-input-dynapro' 'xf86-input-elo2300'
	'xf86-input-jamstudio' 'xf86-input-magellan' 'xf86-input-magictouch' 'xf86-input-microtouch' 'xf86-input-palmax' 'xf86-input-spaceorb' 'xf86-input-summa' 'xf86-input-tek4957' 'xf86-input-ur98' 'xf86-video-vga' 'xf86-video-intel-legacy' 'nvidia-96xx-utils<96.43.14' 'nvidia-173xx-utils<173.14.21')
options=('!libtool')
provides=('x-server')
groups=('xorg')
install=xorg-server.install
source=(${url}/releases/individual/xserver/${pkgname}-${pkgver}.tar.bz2
        xorg-redhat-die-ugly-pattern-die-die-die.patch
        xserver-1.7.1-libcrypto.patch
        xserver-1.7.1-sigaction.patch
        xserver-1.7.1-gamma-kdm-fix.patch
        fix-abi-break.patch
        xvfb-run
        xvfb-run.1)
md5sums=('5c087e0f555203065fd90d02ef5f736e'
         '1a336eb22e27cbf443ec5a2ecddfa93c'
         '957d429cad03ac87281b7e40d963497c'
         '9de9025a8c93b57188fce137b3262d1e'
         '8eae23916552e609c36ecae1827c2e9d'
         'e7e2ed598b96b1bbaf926657db85967e'
         '52fd3effd80d7bc6c1660d4ecf23d31c'
         '376c70308715cd2643f7bff936d9934b')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  # Get rid of the ugly pattern
  patch -Np3 -i "${srcdir}/xorg-redhat-die-ugly-pattern-die-die-die.patch" || return  1

  # http://cvs.fedora.redhat.com/viewvc/rpms/xorg-x11-server/F-12/xserver-1.7.1-libcrypto.patch?view=log
  patch -Np1 -i "${srcdir}/xserver-1.7.1-libcrypto.patch" || return 1

  # http://cvs.fedora.redhat.com/viewvc/rpms/xorg-x11-server/F-12/xserver-1.7.1-sigaction.patch?view=log
  patch -Np1 -i "${srcdir}/xserver-1.7.1-sigaction.patch" || return 1

  # http://cvs.fedora.redhat.com/viewvc/rpms/xorg-x11-server/F-12/xserver-1.7.1-gamma-kdm-fix.patch?view=log
  patch -Np1 -i "${srcdir}/xserver-1.7.1-gamma-kdm-fix.patch" || return 1

  # http://cgit.freedesktop.org/xorg/xserver/commit/?id=155e61a9f0429bf28ce493c0fe7a2d076cb7e137
  patch -Np1 -i "${srcdir}/fix-abi-break.patch" || return 1

  # Fix dbus config path
  sed -i -e 's/\$(sysconfdir)/\/etc/' config/Makefile.*  || return 1

  autoconf || return 1

  ./configure --prefix=/usr \
              --enable-ipv6 \
              --enable-dri \
              --disable-dmx \
              --enable-xvfb \
              --enable-xnest \
              --enable-composite \
              --enable-xcsecurity \
              --enable-xorg \
      	      --enable-xephyr \
	      --enable-glx-tls \
	      --enable-kdrive \
              --enable-install-setuid \
              --enable-config-hal \
      	      --disable-config-dbus \
      	      --disable-record \
      	      --disable-xfbdev \
      	      --disable-xfake \
      	      --disable-xsdl \
              --disable-static \
              --sysconfdir=/etc/X11 \
              --localstatedir=/var \
              --with-default-font-path=/usr/share/fonts/misc,/usr/share/fonts/100dpi:unscaled,/usr/share/fonts/75dpi:unscaled,/usr/share/fonts/TTF,/usr/share/fonts/Type1 \
              --with-xkb-path=/usr/share/X11/xkb \
              --with-xkb-output=/var/lib/xkb \
              --with-dri-driver-path=/usr/lib/xorg/modules/dri || return 1

  make || return 1
  make DESTDIR="${pkgdir}" install || return 1

  install -m755 "${srcdir}/xvfb-run" "${pkgdir}/usr/bin/" || return 1
  install -m644 "${srcdir}/xvfb-run.1" "${pkgdir}/usr/share/man/man1/" || return 1

  rm -rf "${pkgdir}/var/log" || return 1

  install -m755 -d "${pkgdir}/etc/X11" || return 1
  install -m755 -d "${pkgdir}/var/lib/xkb" || return 1

  # Needed for non-mesa drivers, libgl will restore it
  mv "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so" \
     "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.xorg" || return 1

  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/" || return 1
}
