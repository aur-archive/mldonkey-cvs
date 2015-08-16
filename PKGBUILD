# Maintainer: Chen Zhiqiang <chenzhiqiang@mail.com>
# Contributor:  Pierluigi Picciau <pierluigi88@gmail.com>

pkgname=mldonkey-cvs
pkgver=20130722
pkgrel=1
pkgdesc="A multi-network P2P client. CVS version."
arch=('i686' 'x86_64')
url="http://mldonkey.sourceforge.net/"
license=('GPL')
depends=('desktop-file-utils' 'file' 'gd')
makedepends=('lablgtk2' 'librsvg' 'ocaml' 'cvs')
optdepends=('librsvg: GUI support'
            'libx11: GUI support')
backup=()
install=mldonkey.install
source=('mldonkeyd' 'mldonkey.service')
provides=('mldonkey')
md5sums=('1293e6b1f89b0f252974e8947495bcdc' 'df77aa1fbbd96d3e6edbfdcc3b29024f')

_cvsroot=":pserver:anonymous@cvs.savannah.nongnu.org:/sources/mldonkey"
_cvsmod="mldonkey"

_dest=/opt/sourceforge.net/mldonkey

build() {
  cd $srcdir

  msg "Connecting to $_cvsmod.sourceforge.net CVS server...."
  cvs -z3 -d $_cvsroot co -D $pkgver -f $_cvsmod
  cd $_cvsmod
  
  msg "CVS checkout done or server timeout"
  msg "Starting make..."

  rm -fr $srcdir/_build
  cp -r ../$_cvsmod ../_build
  cd ../_build

  ./configure --prefix=$_dest --enable-gui --enable-batch
  make
}

package() {
  cd $srcdir/_build
  make DESTDIR=$pkgdir install

  install -Dm644 icons/rsvg/type_source_normal.svg \
    $pkgdir/$_dest/share/icons/mldonkey.svg
  install -Dm644 distrib/mldonkey.desktop \
    $pkgdir/$_dest/share/applications/mldonkey.desktop
  install -Dm755 $srcdir/mldonkeyd $pkgdir/etc/rc.d/mldonkeyd
  install -Dm644 $srcdir/mldonkey.service $pkgdir/etc/systemd/system/mldonkey.service

  mkdir -p $pkgdir/usr/local/bin
  ln -nsf $_dest/bin/{mlnet,mlgui,mlnet+gui} $pkgdir/usr/local/bin/
  ln -nsf $_dest/bin/mlnet $pkgdir/usr/local/bin/mldonkey
  ln -nsf $_dest/bin/mlgui $pkgdir/usr/local/bin/mldonkey_gui
  ln -nsf $_dest/bin/mlnet+gui $pkgdir/usr/local/bin/mldonkey+gui
  mkdir -p $pkgdir/usr/local/share/{icons,applications}
  ln -nsf $_dest/share/icons/mldonkey.svg $pkgdir/usr/local/share/icons/
  ln -nsf $_dest/share/applications/mldonkey.desktop $pkgdir/usr/local/share/applications/
}

