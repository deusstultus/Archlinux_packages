# $Id$
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Jonathan Wiersma <archaur at jonw dot org>

pkgname=libvirt
pkgver=0.8.8
pkgrel=2
pkgdesc="API for controlling virtualization engines (openvz,kvm,qemu,virtualbox,xen,etc)"
arch=('i686' 'x86_64')
url="http://libvirt.org/"
license=('LGPL')
depends=('e2fsprogs' 'gnutls' 'iptables' 'libxml2' 'parted' 'polkit' 'python2'
	 'avahi' 'yajl' 'libpciaccess' 'udev' 'dbus-core' 'libxau' 'libxdmcp' 'libpcap'
	 'curl' 'libsasl' 'libgcrypt' 'libgpg-error' 'openssl' 'libxcb' 'gcc-libs'
	 'iproute2')
makedepends=('pkgconfig' 'lvm2')
optdepends=('bridge-utils: for briged networking (default)'
	    'dnsmasq: for NAT/DHCP for guests'
	    'kernel26-ovz24: for openvz guests'
	    'kvm'
	    'lxc'
	    'openbsd-netcat: for remote management over ssh'
	    'qemu'
	    'user-mode-linux'
	    'virtualbox_bin'
	    'virtualbox-ose'
	    'xen')
options=('emptydirs' '!libtool')
backup=('etc/conf.d/libvirtd'
	'etc/libvirt/libvirtd.conf'
	'etc/libvirt/qemu.conf'
	'etc/sasl2/libvirt.conf')
install="libvirt.install"
source=("http://libvirt.org/sources/$pkgname-$pkgver.tar.gz"
	libvirtd.rc.d
	libvirtd.conf.d
	openbsd-netcat-default.patch
	unixperms.patch)
md5sums=('ac9235576352b84b8cb17df7456bbdfc'
         '26005f9fe6085e2858f855d94eace5f4'
         'cb4e9bc6b209c1f3077d3698bf1d4437'
         '15bbe9f56644929cee41b8faec0b8d80'
         'db95aecdf2ccf3693fef5821cdcb7eba')

build() {
  cd "$srcdir/$pkgname-$pkgver"

  # python2 fix
  export PYTHON=`which python2`
  for file in $(find . -name '*.py' -print); do
    sed -i 's_#!.*/usr/bin/python_#!/usr/bin/python2_' $file
    sed -i 's_#!.*/usr/bin/env.*python_#!/usr/bin/env python2_' $file
  done

#  for more information on authentication see http://libvirt.org/auth.html
#  If you wish to have unix-permissions based access rather than only polkit
#  access, create a group called libvirt and uncomment the line below:
#  patch -Np1 -i "$srcdir"/unixperms.patch

  [ $NOEXTRACT -eq 1 ] || patch -Np1 -i "$srcdir"/openbsd-netcat-default.patch

  export LDFLAGS=-lX11
  [ -f Makefile ] || ./configure --prefix=/usr --libexec=/usr/lib/"$pkgname" \
	--with-storage-lvm --without-xen --with-udev --without-hal
  find -name Makefile -exec sed -i 's#-L /usr#-L/usr#' {} \;
  make -j1
  make DESTDIR="$pkgdir" install
  install -D -m755 "$srcdir"/libvirtd.rc.d "$pkgdir"/etc/rc.d/libvirtd
  install -D -m644 "$srcdir"/libvirtd.conf.d "$pkgdir"/etc/conf.d/libvirtd
  rm -rf $pkgdir/var/run
}
