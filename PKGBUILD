# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgbase=gtk3
pkgname=(gtk3 gtk-update-icon-cache gtk3-print-backends)
pkgver=3.22.3
pkgrel=1
pkgdesc="GObject-based multi-platform GUI toolkit"
arch=(i686 x86_64)
url="http://www.gtk.org/"
depends=(atk cairo libxcursor libxinerama libxrandr libxi libepoxy gdk-pixbuf2 dconf
         libxcomposite libxdamage pango shared-mime-info at-spi2-atk wayland libxkbcommon
         adwaita-icon-theme json-glib librsvg wayland-protocols desktop-file-utils mesa)
makedepends=(gobject-introspection libcanberra gtk-doc git colord rest libcups)
license=(LGPL)
_commit=99fed96b4470cf02f8fa522551d2a05e01a1bf8a  # tags/3.22.3^0
source=("git://git.gnome.org/gtk+#commit=$_commit"
        0001-gdkscreen-x11-Fix-screen-and-monitor-size-calculatio.patch
        settings.ini
        gtk-query-immodules-3.0.hook
        gtk-update-icon-cache.hook
        gtk-update-icon-cache.script)
sha256sums=('SKIP'
            'f722a70cb1affac8bd054a43b726f57aba21d664144fcaf6d58f18e5bef78189'
            '01fc1d81dc82c4a052ac6e25bf9a04e7647267cc3017bc91f9ce3e63e5eb9202'
            'de46e5514ff39a7a65e01e485e874775ab1c0ad20b8e94ada43f4a6af1370845'
            '496064a9dd6214bd58f689dd817dbdc4d7f17d42a8c9940a87018c3f829ce308'
            'f1d3a0dbfd82f7339301abecdbe5f024337919b48bd0e09296bb0e79863b2541')

pkgver() {
    cd gtk+
    git describe --tags | sed 's/-/+/g'
}

prepare() {
    mkdir print-backends
    cd gtk+
    patch -Np1 -i ../0001-gdkscreen-x11-Fix-screen-and-monitor-size-calculatio.patch
    NOCONFIGURE=1 ./autogen.sh
}

build() {
    cd gtk+

    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --disable-schemas-compile \
        --enable-x11-backend \
        --enable-broadway-backend \
        --enable-wayland-backend \
        --enable-gtk-doc

    #https://bugzilla.gnome.org/show_bug.cgi?id=655517
    sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

    make
}

package_gtk3() {
    depends+=(gtk-update-icon-cache)
    optdepends=('libcanberra: gtk3-widget-factory demo'
                'gtk3-print-backends: Printing')
    install=gtk3.install

    cd gtk+
    make DESTDIR="$pkgdir" install

    install -Dm644 ../settings.ini "$pkgdir/usr/share/gtk-3.0/settings.ini"
    install -Dm644 ../gtk-query-immodules-3.0.hook "$pkgdir/usr/share/libalpm/hooks/gtk-query-immodules-3.0.hook"

    # split this out to use with gtk2 too
    rm "$pkgdir/usr/bin/gtk-update-icon-cache"

    cd "$pkgdir"
    for _f in usr/lib/*/*/printbackends/*; do
        case $_f in
            *-file.so|*-lpr.so) continue;;
        esac

        mkdir -p "$srcdir/print-backends/${_f%/*}"
        mv "$_f" "$srcdir/print-backends/$_f"
    done
}

package_gtk-update-icon-cache() {
    pkgdesc="GTK+ icon cache updater"
    depends=(gdk-pixbuf2 hicolor-icon-theme)

    cd gtk+
    install -D gtk/gtk-update-icon-cache "$pkgdir/usr/bin/gtk-update-icon-cache"
    install -Dm644 ../gtk-update-icon-cache.hook "$pkgdir/usr/share/libalpm/hooks/gtk-update-icon-cache.hook"
    install -D ../gtk-update-icon-cache.script "$pkgdir/usr/share/libalpm/scripts/gtk-update-icon-cache"
}

package_gtk3-print-backends() {
    pkgdesc="Print backends for GTK3"
    depends=(gtk3 rest colord libcups)
    groups=(gnome)

    mv print-backends/* "$pkgdir"
}
# vim:set et sw=4:
