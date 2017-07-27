# $Id$
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgname=gjs
pkgver=1.48.6
pkgrel=1
pkgdesc="Javascript Bindings for GNOME"
url="https://wiki.gnome.org/Projects/Gjs"
arch=(i686 x86_64)
license=(GPL)
depends=(cairo gobject-introspection-runtime js38 gtk3)
makedepends=(gobject-introspection git gnome-common)
_commit=a9db649304db525ca166ec0845ee7a86cea4bf7f  # tags/1.48.6^0
source=("git+https://git.gnome.org/browse/gjs#commit=$_commit")
sha256sums=('SKIP')
options=(debug !strip)

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname
  export CXXFLAGS='-g -O0'
  export CPPFLAGS='-D_FORITFY_SOURCE=0'
  ./configure --prefix=/usr --disable-static --libexecdir=/usr/lib --enable-debug-symbols=-gdwarf-2
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
  make
}

check() {
  cd $pkgname
  # Needs a display
  make -k check || :
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install
}
