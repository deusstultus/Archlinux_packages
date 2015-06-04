# $Id$
# Maintainer: Timothy Redaelli <timothy.redaelli@gmail.com>
# Contributor: shahid <helllamer@gmail.com>

pkgbase=bitcoin
pkgname=('bitcoin-daemon' 'bitcoin-cli' 'bitcoin-qt')
pkgver=0.10.2
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.bitcoin.org/"
makedepends=('boost' 'automoc4' 'qrencode' 'miniupnpc' 'protobuf')
license=('MIT')
source=(http://bitcoin.org/bin/bitcoin-core-$pkgver/bitcoin-$pkgver.tar.gz
	https://raw.github.com/bitcoin/bitcoin/v$pkgver/contrib/debian/bitcoin-qt.desktop
	https://raw.github.com/bitcoin/bitcoin/v$pkgver/share/pixmaps/bitcoin128.png
	https://raw.github.com/bitcoin/bitcoin/v$pkgver/contrib/debian/examples/bitcoin.conf
	https://raw.github.com/bitcoin/bitcoin/v$pkgver/contrib/debian/manpages/bitcoind.1
	https://raw.github.com/bitcoin/bitcoin/v$pkgver/contrib/debian/manpages/bitcoin.conf.5)
sha256sums=('cddf96c71d0a35524fde93380981cf0cf0b51441454a3a68b9be491b9239bfec'
            'b65b377c0d9ecae9eea722843bca0add6bdb7e50929a7e1f751b79b6621c6073'
            'ad880c8459ecfdb96abe6a4689af06bdd27906e0edcd39d0915482f2da91e722'
            'b049cc2650d5988a581d1b72b205254edbaa4f8c0587c2fe8ac4aa0a25543a6a'
            '0a31a5f3ad860ecc1cc8041a863aabdbc4084c7ea5fdc487806b5aa829a244fe'
            '9acf7f46052f6e508af4f38be4574f34bf0f57d2cf462f072606d3f177b4c957')

# Upstream should be more coherent!
case "$pkgver" in
*.*.*.*)
  _pkgver=${pkgver%.*}
  ;;
*)
  _pkgver=$pkgver
  ;;
esac

build() {
  cd "$srcdir/$pkgbase-$pkgver"
  CXXFLAGS="$CXXFLAGS -DBOOST_VARIANT_USE_RELAXED_GET_BY_DEFAULT=1"
  ./configure --prefix=/usr --with-incompatible-bdb --with-gui=qt4
  make
}

package_bitcoin-qt() {
  pkgdesc="Bitcoin is a peer-to-peer network based digital currency - Qt"
  depends=(boost-libs qt4 miniupnpc qrencode protobuf)
  install=bitcoin-qt.install

  cd "$pkgbase-$pkgver"
  install -Dm755 src/qt/bitcoin-qt "$pkgdir"/usr/bin/bitcoin-qt
  install -Dm644 "$srcdir"/bitcoin-qt.desktop \
    "$pkgdir"/usr/share/applications/bitcoin.desktop
  install -Dm644 "$srcdir"/bitcoin128.png \
    "$pkgdir"/usr/share/pixmaps/bitcoin128.png

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

package_bitcoin-daemon() {
  pkgdesc="Bitcoin is a peer-to-peer network based digital currency - daemon"
  depends=(boost-libs miniupnpc openssl)

  cd "$pkgbase-$pkgver"
  install -Dm755 src/bitcoind "$pkgdir"/usr/bin/bitcoind
  install -Dm644 "$srcdir"/bitcoin.conf \
    "$pkgdir/usr/share/doc/$pkgname/examples/bitcoin.conf"
  install -Dm644 "$srcdir"/bitcoind.1 \
    "$pkgdir"/usr/share/man/man1/bitcoind.1
  install -Dm644 "$srcdir"/bitcoin.conf.5 \
    "$pkgdir"/usr/share/man/man5/bitcoin.conf.5
  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

package_bitcoin-cli() {
  pkgdesc="Bitcoin is a peer-to-peer network based digital currency - RPC client"
  depends=(boost-libs openssl)

  cd "$pkgbase-$pkgver"
  install -Dm755 src/bitcoin-cli "$pkgdir"/usr/bin/bitcoin-cli
  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}
