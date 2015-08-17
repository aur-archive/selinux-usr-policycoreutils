# Maintainer: Nicky726 (Nicky726 <at> gmail <dot> com)                 
# Contributor: Sergej Pupykin (pupykin <dot> s+arch <at> gmail <dot> com)
# Contributor: angelux/xangelux (xangelux <at> gmail <dot> com)

pkgname=selinux-usr-policycoreutils
_origname=policycoreutils
_release=20130423
pkgver=2.1.14
pkgrel=2
pkgdesc="SELinux userspace (policycoreutils)"
arch=('i686' 'x86_64')
url="http://userspace.selinuxproject.org"
license=('GPL')
groups=('selinux' 'selinux-userspace')
depends=('selinux-usr-libsemanage>=2.1.0' 'selinux-usr-libselinux>=2.1.0' 'python2'
         'libcap-ng' 'libcgroup' 'dbus-glib' 'python2-ipy' 'selinux-setools')
options=(!emptydirs)
source=(http://userspace.selinuxproject.org/releases/${_release}/${_origname}-${pkgver}.tar.gz
        restorecond.service)
sha256sums=('b6881741f9f9988346a73bfeccb0299941dc117349753f0ef3f23ee86f06c1b5'
            '20572c2cc09c8af5239f26cfea3eb2648d87d9927e55791f13572ea2184e857e')

build() {
  cd "${srcdir}/${_origname}-${pkgver}"

	# optimizations
  sed -i -e "s/-Werror -Wall -W/-Werror -Wall -W ${CFLAGS}/" "setfiles/Makefile"
  sed -i -e "s/-Werror -Wall -W/-Werror -Wall -W ${CFLAGS}/" "sestatus/Makefile"

	# python2
  sed -i -e "s/shell python -c/shell python2 -c/" "semanage/Makefile"
  sed -i -e "s/shell python -c/shell python2 -c/" "sandbox/Makefile"
  sed -i -e "s/\/usr\/bin\/python/\/usr\/bin\/python2/" "sepolicy/Makefile"

	# /bin, /sbin, /usr/sbin -> /usr/bin
  sed -i -e "s/\$(PREFIX)\/sbin/\$(PREFIX)\/bin/g" */Makefile 
  sed -i -e "s/\$(DESTDIR)\/sbin/\$(PREFIX)\/bin/g" */Makefile
  sed -i -e "s/\$(PREFIX)\/sbin/\$(PREFIX)\/bin/g" mcstrans/*/Makefile 
  sed -i -e "s/\$(DESTDIR)\/sbin/\$(PREFIX)\/bin/g" mcstrans/*/Makefile


  make
}

package(){
  cd "${srcdir}/${_origname}-${pkgver}"

  make DESTDIR="${pkgdir}" install

  # Move daemons to correct locations
  mv "${pkgdir}/etc/rc.d/init.d/restorecond" \
        "${pkgdir}/etc/rc.d/"
  sed -i -e "s/init.d\/functions/functions.d/g" \
        "${pkgdir}/etc/rc.d/restorecond"
  rmdir "${pkgdir}/etc/rc.d/init.d/"
  # Install unit file
  install -Dm644 "${srcdir}/restorecond.service" \
  "${pkgdir}/usr/lib/systemd/system/restorecond.service"

  # Following are python2 scripts
  sed -i -e "s/python -E/python2 -E/" \
        "${pkgdir}/usr/bin/audit2allow"
  sed -i -e "s/python -E/python2 -E/" \
        "${pkgdir}/usr/bin/audit2why"
  sed -i -e "s/python -E/python2 -E/" \
        "${pkgdir}/usr/bin/chcat"
  sed -i -e "s/python -E/python2 -E/" \
        "${pkgdir}/usr/bin/sepolgen"
  sed -i -e "s/python -E/python2 -E/" \
        "${pkgdir}/usr/bin/sepolgen-ifgen"
  sed -i -e "s/python -E/python2 -E/" \
        "${pkgdir}/usr/bin/semanage"
  sed -i -e "s/python -E/python2 -E/" \
        "${pkgdir}/usr/lib/python2.7/site-packages/seobject.py"
}

