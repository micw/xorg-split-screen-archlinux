# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.0.1
pkgrel=1
pkgdesc="The GTK+ Toolkit (v3)"
arch=('i686' 'x86_64')
url="http://www.gtk.org/"
install=gtk3.install
depends=('atk' 'cairo' 'gtk-update-icon-cache' 'gnutls' 'heimdal' 'libcups' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'pango' 'shared-mime-info')
makedepends=('gobject-introspection')
options=('!libtool' '!docs')
backup=(etc/gtk-3.0/gtkrc)
license=('LGPL')
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/3.0/gtk+-${pkgver}.tar.bz2)
sha256sums=('2bd0a197dc2a7e981790a720552bfd540a77b6edaa382dd7450366ee23e35121')

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

    echo 'gtk-fallback-icon-theme = "gnome"' > "${pkgdir}/etc/gtk-3.0/gtkrc"
}
