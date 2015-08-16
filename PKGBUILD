# Contributor: Vladimir Kutyavin <vlkut@bk.ru>
pkgname=iptables-l7
pkgver=1.4.7
pkgrel=1
pkgdesc="Iptables with l7-filter support for kernel26-l7"
arch=('i686' 'x86_64')
license=('GPL2')
url="http://www.netfilter.org/"
depends=('glibc' 'bash' 'kernel26-l7' 'l7-protocols')

conflicts=('iptables')
provides=('iptables')

options=('!libtool')
_l7patchsetname="netfilter-layer7"
_l7patchsetver="2.22"
source=(http://www.iptables.org/projects/iptables/files/${pkgname}-${pkgver}.tar.bz2 \
	http://downloads.sourceforge.net/project/l7-filter/l7-filter%20kernel%20version/2.22/netfilter-layer7-v2.22.tar.gz
        iptables ip6tables empty.rules simple_firewall.rules iptables.conf.d)
backup=(etc/conf.d/iptables)
md5sums=('645941dd1f9e0ec1f74c61918d70d52f'
         '98dff8a3d5a31885b73341633f69501f'
         '89401d6f0cf1de46a455b7be6720a58b'
         '6e0e88c2ed0c3715d1409ee3258a0046'
         '14186bbafe21bb0638c0cb8e0903c829'
         'e53a83bb4d8ac8b7eadd7bd58294751d'
         'a9020ee7586417b87c026d435b4be50d')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}

  # http://bugs.archlinux.org/task/17046
  sed -i '87 i libxt_RATEEST.so: libxt_RATEEST.oo' extensions/GNUmakefile.in
  sed -i '88 i \\t${AM_VERBOSE_CCLD} ${CCLD} ${AM_LDFLAGS} -lm -shared ${LDFLAGS} -o $@ $<;\n' extensions/GNUmakefile.in

  # Add layer7 extension to netfilter source
  cp ${srcdir}/${_patchname}-v${_patchver}/iptables-1.4.3forward-for-kernel-2.6.20forward/libxt_layer7.{c,man} ${srcdir}/${pkgname}-${pkgver}/extensions

 ./configure --prefix=/usr --with-kernel=usr/src/linux-$(uname -r) \
	--libexecdir=/usr/lib/iptables --sysconfdir=/etc \
	--with-xtlibdir=/usr/lib/iptables \
	--enable-devel --enable-libipq

  make || return 1
  make DESTDIR=${pkgdir} install || return 1 
  
  install -D -m755 ../iptables ${pkgdir}/etc/rc.d/iptables
  install -D -m755 ../ip6tables ${pkgdir}/etc/rc.d/ip6tables
  install -D -m644 ../empty.rules ${pkgdir}/etc/iptables/empty.rules
  install -D -m644 ../simple_firewall.rules ${pkgdir}/etc/iptables/simple_firewall.rules
  install -D -m644 ../iptables.conf.d ${pkgdir}/etc/conf.d/iptables
}
