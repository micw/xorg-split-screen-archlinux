# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.0.1
pkgrel=2
pkgdesc="The GTK+ Toolkit (v3)"
arch=('i686' 'x86_64')
url="http://www.gtk.org/"
install=gtk3.install
depends=('atk' 'cairo' 'gtk-update-icon-cache' 'gnutls' 'heimdal' 'libcups' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'pango' 'shared-mime-info')
makedepends=('gobject-introspection' 'gtk-doc')
options=('!libtool' '!docs' '!strip')
backup=(etc/gtk-3.0/gtkrc)
license=('LGPL')
#source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/3.0/gtk+-${pkgver}.tar.xz)
source=(gtk+-${pkgver}.tar.xz)
md5sums=('251e5394798ec1ad4762e09e8eebded0')

build() {
    cd "${srcdir}/gtk+-${pkgver}"
    ./autogen.sh
    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --enable-gtk2-dependency \
        --disable-schemas-compile \
        --enable-debug=yes
    make
}

package() {
    cd "${srcdir}/gtk+-${pkgver}"
    make DESTDIR="${pkgdir}" install

    echo 'gtk-fallback-icon-theme = "gnome"' > "${pkgdir}/etc/gtk-3.0/gtkrc"
}
