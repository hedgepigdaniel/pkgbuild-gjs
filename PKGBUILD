# $Id$
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgname=gjs
pkgver=1.48.7
pkgrel=1
pkgdesc="Javascript Bindings for GNOME"
url="https://wiki.gnome.org/Projects/Gjs"
arch=(i686 x86_64)
license=(GPL)
depends=(cairo gobject-introspection-runtime js38 gtk3)
makedepends=(gobject-introspection git gnome-common)
_commit=96a06c8cdf3e2e28d0d77bb7073c004f6ac4c1e6  # tags/1.48.7^0
source=("git+https://git.gnome.org/browse/gjs#commit=$_commit"
	"786186-359381.patch")
sha256sums=('SKIP'
            '1b7aefc4b7ffb5f1e68776616ef8913d3f4ea4f95b18c110d8a0bb0487164ec5')
options=(debug !strip)

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname
  patch -p1 -i ../786186-359381.patch
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
