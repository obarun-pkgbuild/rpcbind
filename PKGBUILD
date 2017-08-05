# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/rpcbind
# 						Maintainer: AndyRTR <andyrtr@archlinux.org>
# 						Maintainer: Tobias Powalowski <tpowa@archlinux.org>

pkgname=rpcbind
pkgver=0.2.4
pkgrel=4
pkgdesc="portmap replacement which supports RPC over various protocols"
arch=(x86_64)
backup=('etc/conf.d/rpcbind')
depends=('bash' 'glibc' 'libtirpc')
optdepends=('rpcbind-runitserv: rpcbind runit service')
url="http://rpcbind.sourceforge.net"
license=('custom')
replaces=('portmap')
# see also http://git.infradead.org/users/steved/rpcbind.git
source=(https://downloads.sourceforge.net/sourceforge/$pkgname/$pkgname-$pkgver.tar.bz2
        rpcbind-sunrpc.patch
        rpcbind.conf
        CVE-2017-8779.diff)
install=rpcbind.install
sha1sums=('e79974a99d09b6d6fff9d86bf00225dc33723ce2'
          '717e2700510b1b40f452eddb929a7be00df4292b'
          '1b997ce76f9727efc8c72fc5f97189591c9a60e2'
          '19a656043d3271b5cbed32b231ce94c28d022905'
          '08246ff18d12cee01e5c9391c6ba8f5597cac936'
          '38044ce8f3376a0b69da8e078e988fd1645bd435')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare() {
  cd $srcdir/${pkgname}-$pkgver
  # patch for iana services file # FS#20273
  patch -Np1 -i ../rpcbind-sunrpc.patch
   # http://seclists.org/oss-sec/2017/q2/209
  patch -Np1 -i ../CVE-2017-8779.diff
}

build() {
  cd $srcdir/${pkgname}-$pkgver
  ./configure --prefix=/usr \
    --with-rpcuser=rpc \
    --enable-warmstarts \
    --with-statedir=/var/lib/rpcbind \
    --with-systemdsystemunitdir=no
  make
}

package() {
  cd $srcdir/${pkgname}-$pkgver
  make DESTDIR=$pkgdir install
  # install rpcbind config file
  install -D -m644 $srcdir/rpcbind.conf $pkgdir/etc/conf.d/rpcbind
  # install missing man page - https://bugs.archlinux.org/task/21271
  install -m644 man/rpcinfo.8 $pkgdir/usr/share/man/man8/
  # add state directory
  install -d -m 700 -o 32 -g 32 $pkgdir/var/lib/rpcbind
  # install license
  install -D -m644 COPYING $pkgdir/usr/share/licenses/rpcbind/COPYING
}
