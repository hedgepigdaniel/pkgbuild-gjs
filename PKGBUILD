# $Id$
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgname=gjs
pkgver=1.48.5
pkgrel=1
pkgdesc="Javascript Bindings for GNOME"
url="https://wiki.gnome.org/Projects/Gjs"
arch=(i686 x86_64)
license=(GPL)
depends=(cairo gobject-introspection-runtime js38 gtk3)
makedepends=(gobject-introspection git gnome-common)
_commit=43c5d7839630dd166372f2c404a9a72c87fd102a  # tags/1.48.5^0
source=(
	"git+https://git.gnome.org/browse/gjs#commit=$_commit"
	"crash.patch::https://bug783935.bugzilla-attachments.gnome.org/attachment.cgi?id=356448&action=diff&collapsed=&context=patch&format=raw&headers=1")
sha256sums=('SKIP' 'SKIP')
options=(debug !strip)

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname
  patch -p1 -i "${srcdir}/crash.patch"
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
