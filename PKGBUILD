# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.0.8
pkgrel=1
pkgdesc="The GTK+ Toolkit (v3)"
arch=('i686' 'x86_64')
url="http://www.gtk.org/"
install=gtk3.install
depends=('atk' 'cairo' 'gtk-update-icon-cache' 'gnutls' 'heimdal' 'libcups' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'pango' 'shared-mime-info')
makedepends=('gobject-introspection')
options=('!libtool' '!docs')
backup=(etc/gtk-3.0/settings.ini)
license=('LGPL')
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/3.0/gtk+-${pkgver}.tar.bz2
        settings.ini)
sha256sums=('42b2b1a400372fe0b3ef890162d1610422f7e787af0765b59f1b3352a4839308'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621')

build() {
    cd "${srcdir}/gtk+-${pkgver}"
    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --enable-gtk2-dependency \
        --disable-schemas-compile
    make
}

package() {
    cd "${srcdir}/gtk+-${pkgver}"
    make DESTDIR="${pkgdir}" install

    install -Dm644 "${srcdir}/settings.ini" "${pkgdir}/etc/gtk-3.0/settings.ini"
}
