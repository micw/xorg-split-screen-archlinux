# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.4.3
pkgrel=1
pkgdesc="GObject-based multi-platform GUI toolkit (v3)"
arch=('i686' 'x86_64')
url="http://www.gtk.org/"
install=gtk3.install
depends=('atk' 'cairo' 'gtk-update-icon-cache' 'libcups' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'pango' 'shared-mime-info' 'colord')
makedepends=('gobject-introspection')
options=('!libtool')
backup=(etc/gtk-3.0/settings.ini)
license=('LGPL')
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/${pkgver%.*}/gtk+-$pkgver.tar.xz
        settings.ini wacom.patch)
sha256sums=('846010442a96590469cde16ed6a1fdfe09397e435165459f04d31cfa5713799f'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621'
            '86bda95a14a99d0f596c4ecb2ed715689f71c207c65dfc90a39d4ae7f1c0c0f5')
build() {
    cd "gtk+-$pkgver"

    # Partially revert BGO#673440 in order to fix BGO#674157
    patch -Np1 -i ../wacom.patch

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
    cd "gtk+-$pkgver"
    make DESTDIR="$pkgdir" install

    install -Dm644 "$srcdir/settings.ini" "$pkgdir/etc/gtk-3.0/settings.ini"
}
