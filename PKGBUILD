# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.2.3
pkgrel=2
pkgdesc="GTK+ is a multi-platform toolkit (v3)"
arch=('i686' 'x86_64')
url="http://www.gtk.org/"
install=gtk3.install
depends=('atk' 'cairo' 'gtk-update-icon-cache' 'libcups' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'pango' 'shared-mime-info' 'colord')
makedepends=('gobject-introspection')
options=('!libtool' '!docs')
backup=(etc/gtk-3.0/settings.ini)
license=('LGPL')
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/3.2/gtk+-$pkgver.tar.xz
        settings.ini
        empty_grid.patch)
sha256sums=('e2cf20f2510ebbc7be122a1a33dd1f472a7d06aaf16b4f2a63eb048cd9141d3d'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621'
            'd05ccfeaf4c558668b72aaacdd11356b6419d2359def6c1b9af1b465fa5a3c25')

build() {
    cd "$srcdir/gtk+-$pkgver"
    patch -Np1 -i "$srcdir/empty_grid.patch"
    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --enable-gtk2-dependency \
        --disable-schemas-compile
    #https://bugzilla.gnome.org/show_bug.cgi?id=655517
    sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
    make
}

package() {
    cd "$srcdir/gtk+-$pkgver"
    make DESTDIR="$pkgdir" install

    install -Dm644 "$srcdir/settings.ini" "$pkgdir/etc/gtk-3.0/settings.ini"
}
