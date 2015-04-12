# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgname=gtk3
pkgver=3.16.1
pkgrel=2
pkgdesc="GObject-based multi-platform GUI toolkit (v3)"
arch=(i686 x86_64)
url="http://www.gtk.org/"
install=gtk3.install
depends=(atk cairo gtk-update-icon-cache libcups libxcursor libxinerama libxrandr libxi libepoxy
         libxcomposite libxdamage pango shared-mime-info colord at-spi2-atk wayland libxkbcommon
         adwaita-icon-theme json-glib rest)
makedepends=(gobject-introspection)
license=(LGPL)
source=(https://download.gnome.org/sources/gtk+/${pkgver:0:4}/gtk+-$pkgver.tar.xz
        0001-Revert-image-Optimize-non-resize-changes.patch
        0001-x11-Relax-requirements-for-setting-ParentRelative.patch)
sha256sums=('9f9716a8c7f8dc149f767718e25fed41984b504a00fd0919b1a4969ca2e4db31'
            '4e21dd47f520f7a4c5143f34b208c0a24f0bf620d5409d45dbe2901bfa7d5e9a'
            '94ec473293946e45217ed35e9518c45771bf28f1007962bb749e21906bf0201b')

prepare() {
    cd gtk+-$pkgver

    # revert commit that causes nm-applet to fail to load
    # https://bugzilla.redhat.com/show_bug.cgi?id=1208183
    patch -Np1 -i ../0001-Revert-image-Optimize-non-resize-changes.patch

    # fix transparency setting for status icons on Xfce
    # patch not yet upstream but it should get accepted
    # https://bugzilla.gnome.org/show_bug.cgi?id=747524
    patch -Np1 -i ../0001-x11-Relax-requirements-for-setting-ParentRelative.patch
}

build() {
    cd "gtk+-$pkgver"

    CXX=/bin/false ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --disable-schemas-compile \
        --enable-x11-backend \
        --enable-broadway-backend \
        --enable-wayland-backend

    #https://bugzilla.gnome.org/show_bug.cgi?id=655517
    sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

    make
}

package() {
    cd "gtk+-$pkgver"
    make DESTDIR="$pkgdir" install
    rm -f "$pkgdir/usr/bin/gtk-update-icon-cache"
}
