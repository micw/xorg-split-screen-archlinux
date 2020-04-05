# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgbase=gtk3
pkgname=(gtk3 gtk-update-icon-cache)
pkgver=3.24.17+4+g8c97702a07
pkgrel=1
epoch=1
pkgdesc="GObject-based multi-platform GUI toolkit"
arch=(x86_64)
url="https://www.gtk.org/"
depends=(atk cairo libxcursor libxinerama libxrandr libxi libepoxy gdk-pixbuf2 dconf
         libxcomposite libxdamage pango shared-mime-info at-spi2-atk wayland libxkbcommon
         adwaita-icon-theme json-glib librsvg wayland-protocols desktop-file-utils mesa
         cantarell-fonts colord rest libcups libcanberra fribidi iso-codes)
makedepends=(gobject-introspection gtk-doc git glib2-docs sassc meson)
license=(LGPL)
_commit=8c97702a0771ccb186088ea04478dfc8c6616fe4  # gtk-3-24
source=("git+https://gitlab.gnome.org/GNOME/gtk.git#commit=$_commit"
        1633.patch
        settings.ini
        gtk-query-immodules-3.0.hook
        gtk-update-icon-cache.hook
        gtk-update-icon-cache.script)
sha256sums=('SKIP'
            '7b7141c53de03d0bd6c73287b27c75e39a8eede1853037f52183c4648805255d'
            '01fc1d81dc82c4a052ac6e25bf9a04e7647267cc3017bc91f9ce3e63e5eb9202'
            'a0319b6795410f06d38de1e8695a9bf9636ff2169f40701671580e60a108e229'
            '2d435e3bec8b79b533f00f6d04decb1d7c299c6e89b5b175f20be0459f003fe8'
            'f1d3a0dbfd82f7339301abecdbe5f024337919b48bd0e09296bb0e79863b2541')

pkgver() {
  cd gtk
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd gtk

  # https://bugs.archlinux.org/task/66106
  # https://gitlab.gnome.org/GNOME/gtk/-/merge_requests/1633
  git apply -3 ../1633.patch

  # https://bugs.archlinux.org/task/66119
  # https://gitlab.gnome.org/GNOME/gtk/-/issues/2576
  # no patch yet
}

build() {
  CFLAGS+=" -DG_ENABLE_DEBUG -DG_DISABLE_CAST_CHECKS"
  arch-meson gtk build \
    -D broadway_backend=true \
    -D colord=yes \
    -D gtk_doc=true \
    -D man=true
  ninja -C build
}

package_gtk3() {
  depends+=(gtk-update-icon-cache)
  provides=(gtk3-print-backends libgtk-3.so libgdk-3.so libgailutil-3.so)
  conflicts=(gtk3-print-backends)
  replaces=("gtk3-print-backends<=3.22.26-1")
  install=gtk3.install

  DESTDIR="$pkgdir" meson install -C build

  install -Dt "$pkgdir/usr/share/gtk-3.0" -m644 settings.ini
  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 gtk-query-immodules-3.0.hook

  # split this out to use with gtk2 too
  mv "$pkgdir/usr/bin/gtk-update-icon-cache" "$srcdir"
}

package_gtk-update-icon-cache() {
  pkgdesc="GTK+ icon cache updater"
  depends=(gdk-pixbuf2 librsvg hicolor-icon-theme)

  install -Dt "$pkgdir/usr/bin" gtk-update-icon-cache
  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 gtk-update-icon-cache.hook
  install -D gtk-update-icon-cache.script "$pkgdir/usr/share/libalpm/scripts/gtk-update-icon-cache"
}

# vim:set ts=2 sw=2 et:
