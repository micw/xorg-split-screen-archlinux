# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.0.10
pkgrel=2
pkgdesc="The GTK+ Toolkit (v3)"
arch=('i686' 'x86_64')
url="http://www.gtk.org/"
install=gtk3.install
depends=('atk' 'cairo' 'gtk-update-icon-cache' 'gnutls' 'krb5' 'libcups' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'pango' 'shared-mime-info')
makedepends=('gobject-introspection')
options=('!libtool' '!docs')
backup=(etc/gtk-3.0/settings.ini)
license=('LGPL')
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/3.0/gtk+-${pkgver}.tar.bz2
        settings.ini
        only_draw_resize_grip_if_its_window_exists.patch)
sha256sums=('a217ed31aed9843810eabb241239e0f2c5ca0e14bd91687cf4180b3e346abc30'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621'
            'fa2bd8085cae6cdb4f3b8baae11d3c0b38afec2eb4503f5bacabf87aee15313a')

build() {
    cd "${srcdir}/gtk+-${pkgver}"
    patch -Np1 -i "${srcdir}/only_draw_resize_grip_if_its_window_exists.patch"
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
