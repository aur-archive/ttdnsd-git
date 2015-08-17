## Original contributor: Nicolas Pouillard <nicolas.pouillard@gmail.com>
## Maintainer: Nathan "Necopinus" <nathan@ecopunk.info>

pkgname=ttdnsd-git
pkgver=20111022
pkgrel=3
pkgdesc="Tor TCP DNS Daemon"
arch=('i686' 'x86_64')
# This is the URL in the README, but it is currently broken:
# url="https://www.torproject.org/ttdnsd/"
url="https://gitweb.torproject.org/ioerror/ttdnsd.git"
license=(custom:BSD3)
depends=(tor tsocks)
backup=('etc/conf.d/ttdnsd' 'etc/ttdnsd.conf')
source=('ttdnsd.rcd'
        'ttdnsd.service')
sha256sums=('76d11e577df62708076e71654bb3da2007c28ec3f92e95ec90a2b9e035012a3f'
            '08943ab5d6ddbd32b877a99dfdd8df52ce78e153e20133f8d9069e4a76f613ad')

_gitroot='https://git.torproject.org/ioerror/ttdnsd.git'
_gitrepo='ttdnsd'

build() {
  cd $srcdir

  if [ -d $_gitrepo ]; then
    (cd $_gitrepo && git checkout HEAD && git pull .) || return 1
  else
    git clone $_gitroot $_gitrepo || return 1
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  [ -d $_gitrepo-build ] && rm -rf $_gitrepo-build
  cp -r $_gitrepo $_gitrepo-build
  cd $_gitrepo-build

  make || return 1
}

package() {
  cd $srcdir/$_gitrepo-build
  make DESTDIR=$pkgdir install

  mv $pkgdir/etc/default $pkgdir/etc/conf.d
  rm -r $pkgdir/etc/init.d

  sed -i -e 's#/etc/default#/etc/conf.d#g' $pkgdir/etc/conf.d/ttdnsd
  chmod 644 $pkgdir/etc/conf.d/ttdnsd

  mkdir -p $pkgdir/etc/rc.d
  install -Dm755 $srcdir/ttdnsd.rcd $pkgdir/etc/rc.d/ttdnsd

  mkdir -p $pkgdir/lib/systemd/system
  install -Dm644 $srcdir/ttdnsd.service $pkgdir/lib/systemd/system/ttdnsd.service
}
