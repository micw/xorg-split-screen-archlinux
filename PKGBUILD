# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgname=gtk3
pkgver=3.10.1
pkgrel=2
pkgdesc="GObject-based multi-platform GUI toolkit (v3)"
arch=(i686 x86_64)
url="http://www.gtk.org/"
install=gtk3.install
depends=(atk cairo gtk-update-icon-cache libcups libxcursor libxinerama libxrandr libxi
         libxcomposite libxdamage pango shared-mime-info colord at-spi2-atk wayland libxkbcommon)
makedepends=(gobject-introspection)
optdepends=('gnome-themes-standard: Default widget theme'
            'gnome-icon-theme: Default icon theme')
options=('!libtool')
license=(LGPL)
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/${pkgver%.*}/gtk+-$pkgver.tar.xz
        settings.ini gtkicontheme-double-free.patch)
sha256sums=('c12e6897fb1ec8d8f1a6de6cd0ac1372fee6fd63ee3a5a63813dc5f3473e6ab8'
            '14369dfd1d325c393e17c105d5d5cc5501663277bd4047ea04a50abb3cfbd119'
            '587be357826a386b12758c899b8c47a7906c90585e6ddaddc848537744b36771')

prepare() {
    cd "gtk+-$pkgver"
    patch -Np1 -i ../gtkicontheme-double-free.patch
}

build() {
    cd "gtk+-$pkgver"

    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --enable-gtk2-dependency \
        --disable-schemas-compile \
        --enable-x11-backend \
        --enable-broadway-backend \
        --enable-wayland-backend

    #https://bugzilla.gnome.org/show_bug.cgi?id=655517
    sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

    make
}

package() {
    cd "gtk+-$pkgver"
    make DESTDIR="$pkgdir" install
    install -Dm644 ../settings.ini "$pkgdir/usr/share/gtk-3.0/settings.ini"
}
