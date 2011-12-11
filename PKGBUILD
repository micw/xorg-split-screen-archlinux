# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.2.2
pkgrel=4
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
        filechooserdefault_do_not_unref_value_twice.patch
        moveresize.patch)
sha256sums=('f7ec82de393cd7ae2aa45022576400941704709d1f0f35fb0b17f3be1f2e7d84'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621'
            '0d6b04d5fc12b7c08e0cff4b94d001d5c167a944b72579fb14fd6de2ee4ad9e6'
            '627f430ccecc95ec61d9a83607469e71e95568c077f7bda849a7fe1c02d0bea7')

build() {
    cd "$srcdir/gtk+-$pkgver"
    patch -Np1 -i "$srcdir/filechooserdefault_do_not_unref_value_twice.patch"
    patch -Np1 -i "$srcdir/moveresize.patch"
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
