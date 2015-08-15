# Maintainer: vsalmena < vincenzosalmena at gmail dot com >
# Contributor: grvrulz < grvrulz at conetfun dot com >

pkgname=gazette-bzr
pkgver=92
pkgrel=1
pkgdesc="Current news in classic columns"
arch=('i686' 'x86_64')
url="https://launchpad.net/gazette"
license=('GPL3')
groups=('pantheon')
depends=('granite-bzr' 'libsoup' 'clutter-gtk' 'zeitgeist' 'libzeitgeist' 'libpantheon-bzr' 'libgdata')
optdepends=( 'switchboard-bzr: Gazette plug for Switchboard' )
makedepends=('bzr' 'cmake' 'vala0.18')
conflicts=('gazette')
provides=('gazette')
install=gazette.install

_bzrtrunk=lp:gazette
_bzrmod=gazette

build() {
  msg "Connecting to Bazaar server...."

  if [ -d "$_bzrmod" ]; then
    cd "$_bzrmod" && bzr pull "$_bzrtrunk" && cd ..
    msg "The local files are updated."
  else
    bzr branch "$_bzrtrunk" "$_bzrmod" -r "$pkgver"
  fi

  msg "BZR checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$_bzrmod-build"
  cp -rf "$_bzrmod" "$_bzrmod-build"
  cd "$_bzrmod-build"
  rm -rf build/
  mkdir build
  cd build
  cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DGSETTINGS_COMPILE=OFF -DVALA_EXECUTABLE="$(type -p valac-0.18)"
  make
}

package() {
  cd "$srcdir/$_bzrmod-build/build"
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="$pkgdir/" install
}

