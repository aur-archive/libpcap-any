# $Id: PKGBUILD 20200 2008-12-01 17:56:56Z thomas $
# Maintainer: Thomas Bächler <thomas@archlinux.org>

pkgname=libpcap-any
_pkgname=libpcap
pkgver=1.0.0
pkgrel=1
pkgdesc="A system-independent interface for user-level packet capture with debian patch (-i any)"
arch=('i686' 'x86_64')
url="http://www.tcpdump.org/"
license=('BSD')
groups=('base')
depends=('glibc')
makedepends=('flex')
conflicts=(libpcap)
replaces=(libpcap)
source=(http://www.tcpdump.org/release/libpcap-${pkgver}.tar.gz 
	20-fix-any-intf.diff
	30_man_fixes.diff)
md5sums=('9ad1358c5dec48456405eac197a46d3d')

build() {
  cd ${srcdir}/${_pkgname}-${pkgver}
  patch -p1 < ../20-fix-any-intf.diff
  patch -p1 < ../30_man_fixes.diff
  ./configure --prefix=/usr --enable-ipv6
  make || return 1
  make shared || return 1
  
  install -d -m755 ${pkgdir}/usr/bin
  make DESTDIR=${pkgdir} install install-shared || return 1
  ln -s libpcap.so.1.0.0 ${pkgdir}/usr/lib/libpcap.so.1
  ln -s libpcap.so.1.0.0 ${pkgdir}/usr/lib/libpcap.so
  # backwards compatibility, programs often look for net/bpf.h
  mkdir -p ${pkgdir}/usr/include/net
  cd ${pkgdir}/usr/include/net
  ln -s ../pcap-bpf.h bpf.h

  #install the license
  install -D -m644 ${srcdir}/$_pkgname-$pkgver/LICENSE ${pkgdir}/usr/share/licenses/$pkgname/LICENSE
}
