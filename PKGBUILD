# Maintainer: Leo Bärring <leo.barring@gmail.com>
# some parts copied from pcb package
pkgname=pcb-git
pkgver=20130622
pkgrel=1
pkgdesc="An interactive printed circuit board editor for the X11 window system."
arch=('any')
url="http://pcb.geda-project.org"
license=('GPL')
groups=()
depends=('dbus' 'gtkglext' 'gd')
makedepends=('git' 'intltool' 'texlive-core' 'ghostscript' 'mesa')
optdepends=('tk: for the graphical QFP footprint builder')
provides=('pcb')
conflicts=('pcb')
replaces=()
backup=()
options=()
install=$pkgname.install
source=()
noextract=()
md5sums=()

_gitroot="git://git.geda-project.org/pcb.git"
_gitname="pcb"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull --depth=1 origin
    msg "The local files are updated."
  else
    git clone --depth=1 $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  cp -r "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #
  # BUILD HERE
  #

  if [ ! -x /usr/bin/wish ]; then
     config="env WISH=/usr/bin/true ./configure --enable-dbus"
  else
     config="./configure --enable-dbus"
  fi

  ./autogen.sh
  $config --prefix=/usr --disable-update-mime-database --disable-update-desktop-database

  make
}

package() {
  cd "$srcdir/$_gitname-build"
  make -j1 prefix=$pkgdir/usr install
  rm $pkgdir/usr/share/info/dir 
} 
