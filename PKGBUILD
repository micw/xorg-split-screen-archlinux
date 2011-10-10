# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.2.0
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
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/3.2/gtk+-${pkgver}.tar.xz
        settings.ini
        a11y.patch::http://git.gnome.org/browse/gtk+/patch/?id=e248c6812e8e33150d61074471ef0330668aed45)
sha256sums=('bce3c1a9be6afd7552c795268656d8fdd09c299765a7faaf5a76498bb82ed44c'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621'
            '0ae5f9c3553f9fc6c515343de96046c17544654936b17c09330443ed44778cb2')

build() {
    cd "${srcdir}/gtk+-${pkgver}"
    patch -Np1 -i "${srcdir}/a11y.patch"
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
    cd "${srcdir}/gtk+-${pkgver}"
    make DESTDIR="${pkgdir}" install

    install -Dm644 "${srcdir}/settings.ini" "${pkgdir}/etc/gtk-3.0/settings.ini"
}
