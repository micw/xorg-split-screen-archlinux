# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgbase=gtk3
pkgname=(gtk3 gtk3-docs gtk3-demos)
pkgver=3.24.35
pkgrel=1
epoch=1
pkgdesc="GObject-based multi-platform GUI toolkit"
url="https://www.gtk.org/"
arch=(x86_64)
license=(LGPL)
depends=(
  adwaita-icon-theme
  at-spi2-atk
  atk
  cairo
  cantarell-fonts
  dconf
  desktop-file-utils
  fribidi
  gdk-pixbuf2
  gtk-update-icon-cache
  iso-codes
  libcloudproviders
  libcolord
  libcups
  libepoxy
  librsvg
  libxcomposite
  libxcursor
  libxdamage
  libxi
  libxinerama
  libxkbcommon
  libxrandr
  mesa
  pango
  shared-mime-info
  tracker3
  wayland
)
makedepends=(
  git
  glib2-docs
  gobject-introspection
  gtk-doc
  meson
  sassc
  wayland-protocols
)
options=(debug)
_commit=14cf55f98ddd71ad3f91487eda1c7f14d67de119  # tags/3.24.35^0
source=(
  "git+https://gitlab.gnome.org/GNOME/gtk.git#commit=$_commit"
  gtk-query-immodules-3.0.hook
)
sha256sums=('SKIP'
            'a0319b6795410f06d38de1e8695a9bf9636ff2169f40701671580e60a108e229')

pkgver() {
  cd gtk
  git describe --tags | sed 's/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd gtk
}

build() {
  local meson_options=(
    -D broadway_backend=true
    -D cloudproviders=true
    -D colord=yes
    -D gtk_doc=true
    -D introspection=true
    -D man=true
    -D tracker3=true
  )

  CFLAGS+=" -DG_DISABLE_CAST_CHECKS"
  arch-meson gtk build "${meson_options[@]}"
  meson compile -C build
}

_pick() {
  local p="$1" f d; shift
  for f; do
    d="$srcdir/$p/${f#$pkgdir/}"
    mkdir -p "$(dirname "$d")"
    mv "$f" "$d"
    rmdir -p --ignore-fail-on-non-empty "$(dirname "$f")"
  done
}

package_gtk3() {
  optdepends=('evince: Default print preview command')
  provides=(gtk3-print-backends libgtk-3.so libgdk-3.so libgailutil-3.so)
  conflicts=(gtk3-print-backends)
  replaces=("gtk3-print-backends<=3.22.26-1")
  install=gtk3.install

  meson install -C build --destdir "$pkgdir"

  install -Dm644 /dev/stdin "$pkgdir/usr/share/gtk-3.0/settings.ini" <<END
[Settings]
gtk-icon-theme-name = Adwaita
gtk-theme-name = Adwaita
gtk-font-name = Cantarell 11
END

  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 gtk-query-immodules-3.0.hook

  cd "$pkgdir"

  rm usr/bin/gtk-update-icon-cache
  rm usr/share/man/man1/gtk-update-icon-cache.1

  _pick docs usr/share/gtk-doc

  _pick demo usr/bin/gtk3-{demo,demo-application,icon-browser,widget-factory}
  _pick demo usr/share/applications/gtk3-{demo,icon-browser,widget-factory}.desktop
  _pick demo usr/share/glib-2.0/schemas/org.gtk.{Demo,exampleapp}.gschema.xml
  _pick demo usr/share/icons/hicolor/*/apps/gtk3-{demo,widget-factory}[-.]*
  _pick demo usr/share/man/man1/gtk3-{demo,demo-application,icon-browser,widget-factory}.1
}

package_gtk3-docs() {
  pkgdesc+=" (documentation)"
  depends=()
  mv docs/* "$pkgdir"
}

package_gtk3-demos() {
  pkgdesc+=" (demo applications)"
  depends=(gtk3)
  mv demo/* "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
