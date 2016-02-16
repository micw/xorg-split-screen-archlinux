# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgbase=gtk3
pkgname=(gtk3 gtk-update-icon-cache)
pkgver=3.18.7
pkgrel=2
pkgdesc="GObject-based multi-platform GUI toolkit"
arch=(i686 x86_64)
url="http://www.gtk.org/"
depends=(atk cairo libcups libxcursor libxinerama libxrandr libxi libepoxy gdk-pixbuf2
         libxcomposite libxdamage pango shared-mime-info colord at-spi2-atk wayland libxkbcommon
         adwaita-icon-theme json-glib rest librsvg)
makedepends=(gobject-introspection libcanberra gtk-doc)
license=(LGPL)
source=(https://download.gnome.org/sources/gtk+/${pkgver:0:4}/gtk+-$pkgver.tar.xz
        0001-file-chooser-Avoid-a-crash.patch
        0001-file-chooser-Use-the-right-model-when-getting-the-se.patch
        settings.ini)
sha256sums=('a76e1cb0ac45ce5c2734f6778f2731a5c6a23d1ff6bd4db357774f014ee68820'
            '36058e522839bcae775cbafef8d4ad21cba98265b5cf9e90d5b46b874ddbf79f'
            '22c0272fdbb048ec8fe62d71bcb1ab6765050de3eb04e833ba7303750782d0a7'
            '01fc1d81dc82c4a052ac6e25bf9a04e7647267cc3017bc91f9ce3e63e5eb9202')

prepare() {
    cd gtk+-$pkgver
    NOCONFIGURE=1 ./autogen.sh

    # https://bugzilla.gnome.org/show_bug.cgi?id=761209
    patch -Np1 -i ../0001-file-chooser-Avoid-a-crash.patch

    # https://bugzilla.gnome.org/show_bug.cgi?id=761757
    patch -Np1 -i ../0001-file-chooser-Use-the-right-model-when-getting-the-se.patch
}

build() {
    cd "gtk+-$pkgver"

    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --disable-schemas-compile \
        --enable-x11-backend \
        --enable-broadway-backend \
        --enable-wayland-backend

    #https://bugzilla.gnome.org/show_bug.cgi?id=655517
    sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

    make
}

package_gtk3() {
    depends+=(gtk-update-icon-cache)
    optdepends=('libcanberra: gtk3-widget-factory demo')
    install=gtk3.install

    cd "gtk+-$pkgver"
    make DESTDIR="$pkgdir" install
    install -Dm644 ../settings.ini "$pkgdir/usr/share/gtk-3.0/settings.ini"

    # split this out to use with gtk2 too
    rm "$pkgdir/usr/bin/gtk-update-icon-cache"
}

package_gtk-update-icon-cache() {
    pkgdesc="GTK+ icon cache updater"
    depends=(gdk-pixbuf2 hicolor-icon-theme)
    install=gtk-update-icon-cache.install

    cd gtk+-$pkgver/gtk
    install -Dm755 gtk-update-icon-cache "$pkgdir/usr/bin/gtk-update-icon-cache"
}

# vim:set et sw=4:
