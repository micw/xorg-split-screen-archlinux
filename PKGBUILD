# Maintainer: Ionut Biru <ibiru@archlinux.org>

pkgname=gtk3
pkgver=3.2.2
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
        fix_crash_treeview.patch)
sha256sums=('f7ec82de393cd7ae2aa45022576400941704709d1f0f35fb0b17f3be1f2e7d84'
            'c214d3dcdcadda3d642112287524ab3e526ad592b70895c9f3e3733c23701621'
            'eb187ef05f3735b24b58cd4c533dc3d4f616103cafba399c009ea3eb519be0b9')

build() {
    cd "$srcdir/gtk+-$pkgver"
    patch -Np1 -R -i "$srcdir/fix_crash_treeview.patch"
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
