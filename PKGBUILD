# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgname=gtk3
pkgver=3.14.8
pkgrel=1
pkgdesc="GObject-based multi-platform GUI toolkit (v3)"
arch=(i686 x86_64)
url="http://www.gtk.org/"
install=gtk3.install
depends=(atk cairo gtk-update-icon-cache libcups libxcursor libxinerama libxrandr libxi
         libxcomposite libxdamage pango shared-mime-info colord at-spi2-atk wayland libxkbcommon
         adwaita-icon-theme json-glib rest)
makedepends=(gobject-introspection python2)
license=(LGPL)
source=(https://download.gnome.org/sources/gtk+/${pkgver:0:4}/gtk+-$pkgver.tar.xz)
sha256sums=('7a543ea8a85f2c8a7b7374f24fd57df454067858689d8b02ec6c90f24be2b344')

prepare() {
    cd gtk+-$pkgver
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
}
