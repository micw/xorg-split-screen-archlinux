# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.8.2
pkgrel=1
pkgdesc="GObject-based multi-platform GUI toolkit (v3)"
arch=(i686 x86_64)
url="http://www.gtk.org/"
install=gtk3.install
depends=(atk cairo gtk-update-icon-cache libcups libxcursor libxinerama libxrandr libxi libxcomposite libxdamage pango shared-mime-info colord at-spi2-atk wayland libxkbcommon)
makedepends=(gobject-introspection)
options=('!libtool')
backup=(etc/gtk-3.0/settings.ini)
license=(LGPL)
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/${pkgver%.*}/gtk+-$pkgver.tar.xz
        settings.ini)
sha256sums=('1ca80c9c15a1df95d74cefb8c2afe4682ba272a4b489106f04877be2a7aff297'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621')
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

    install -Dm644 "$srcdir/settings.ini" "$pkgdir/etc/gtk-3.0/settings.ini"
}
