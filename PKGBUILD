# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgbase=gtk3
pkgname=(gtk3 gtk-update-icon-cache)
pkgver=3.20.4
pkgrel=1
pkgdesc="GObject-based multi-platform GUI toolkit"
arch=(i686 x86_64)
url="http://www.gtk.org/"
depends=(atk cairo libcups libxcursor libxinerama libxrandr libxi libepoxy gdk-pixbuf2
         libxcomposite libxdamage pango shared-mime-info colord at-spi2-atk wayland libxkbcommon
         adwaita-icon-theme json-glib rest librsvg wayland-protocols desktop-file-utils)
makedepends=(gobject-introspection libcanberra gtk-doc)
license=(LGPL)
source=(https://download.gnome.org/sources/gtk+/${pkgver:0:4}/gtk+-$pkgver.tar.xz
        settings.ini
        gtk-query-immodules-3.0.hook
        gtk-update-icon-cache.hook
        gtk-update-icon-cache.script)
sha256sums=('e7e3aaf54a54dd1c1ca0588939254abe31329e0bcd280a12290d5306b41ea03f'
            '01fc1d81dc82c4a052ac6e25bf9a04e7647267cc3017bc91f9ce3e63e5eb9202'
            'de46e5514ff39a7a65e01e485e874775ab1c0ad20b8e94ada43f4a6af1370845'
            '496064a9dd6214bd58f689dd817dbdc4d7f17d42a8c9940a87018c3f829ce308'
            'bbe06e1b4e1ad5d61a4e703445a2bb93c6be918964d6dd76c0420c6667fa11eb')

prepare() {
    cd gtk+-$pkgver
    NOCONFIGURE=1 ./autogen.sh
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
    install -Dm644 ../gtk-query-immodules-3.0.hook "$pkgdir/usr/share/libalpm/hooks/gtk-query-immodules-3.0.hook"

    # split this out to use with gtk2 too
    rm "$pkgdir/usr/bin/gtk-update-icon-cache"
}

package_gtk-update-icon-cache() {
    pkgdesc="GTK+ icon cache updater"
    depends=(gdk-pixbuf2 hicolor-icon-theme)

    cd gtk+-$pkgver
    install -D gtk/gtk-update-icon-cache "$pkgdir/usr/bin/gtk-update-icon-cache"
    install -Dm644 ../gtk-update-icon-cache.hook "$pkgdir/usr/share/libalpm/hooks/gtk-update-icon-cache.hook"
    install -D ../gtk-update-icon-cache.script "$pkgdir/usr/share/libalpm/scripts/gtk-update-icon-cache"
}

# vim:set et sw=4:
