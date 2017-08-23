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
source=("git+https://git.gnome.org/browse/gjs#commit=$_commit"
	"785657-358257.patch"
	"785657-358258.patch"
	"785657-358259.patch")
sha256sums=('SKIP'
            'f8f6edde8cc8a55c2518cb5767cfb95b9dc56400910add753ff190497ee5265c'
            'dd900a893477f24d9a041ca7dc4261850034db56d908ce6d1e11aaba212b4d77'
            'c112e8d8b2862dcb9ba902620677039388607f19ef95c90766679a5f8f052f92')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname
  patch -p1 -i ../785657-358257.patch
  patch -p1 -i ../785657-358258.patch
  patch -p1 -i ../785657-358259.patch
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname
  ./configure --prefix=/usr --disable-static --libexecdir=/usr/lib
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
