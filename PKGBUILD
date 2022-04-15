# Maintainer: Frede Hundewadt <fh@manjaro.org>
# Contributor: Philip Müller <philm@manjaro.org>

# test package branch
_branch=master
#_branch=v4.21.x-stable
_date=$(date +%Y%m%d)

pkgname=pacman-mirrors
pkgver=4.23.2
pkgrel=3
pkgdesc="Manjaro Linux mirror list for use by pacman"
arch=('any')
depends=('python' 'python-npyscreen' 'python-requests' 'python-certifi')
makedepends=('git' 'python-babel' 'python-build' 'python-installer'
             'python-setuptools' 'python-wheel')
optdepends=('gtk3: for interactive mode (GUI)'
            'python-gobject: for interactive mode (GUI)')
# test package source
#url="https://codeberg.org/fhdk/pacman-mirrors"
url="https://gitlab.manjaro.org/applications/pacman-mirrors"
conflicts=('pacman-mirrorlist' 'pacman-mirrors-dev' 'maint' 'reflector')
replaces=('pacman-mirrorlist')
provides=("pacman-mirrorlist=${_date}" "${pkgname}=$pkgver")
license=('GPL')
#backup=("etc/${pkgname}.conf") ==> WARNING: backup entry file not in package : etc/pacman-mirrors.conf
source=("git+$url.git#branch=${_branch}"
        "${pkgname}-install.script"
        "${pkgname}-upgrade.script"
        "${pkgname}-install.hook"
        "${pkgname}-upgrade.hook")
sha256sums=('SKIP'
            '718a47605be1ca328255b19047dee6d331e0440f303b86d17485fe53937b7906'
            '3b1df8c662161903653b0ae41d910019f87a58f3ecd8e02ea9ac8859b9c43f17'
            '88befb1a9b167112e05544ec4a765705bf474209e7ef67c44ffc418e10e89bfa'
            '6b6869d9dd85cd3a3cba49013dd2fc1c5f7a0934ba1284e21d4bbd24fa2540c6')

prepare() {
  cd "${srcdir}/${pkgname}"
  # do something here
}

build() {
  cd "${srcdir}/${pkgname}"
  make clean
  make mo-files
  python -m build --wheel --no-isolation
}

package() {
  cd "${srcdir}/${pkgname}"
  # make DESTDIR="${pkgdir}" install
  python -m installer --destdir="${pkgdir}" dist/*.whl

  # Automatically generated, do we actually need to install it?
#  install -Dm644 conf/${pkgname}.conf "${pkgdir}/etc/${pkgname}.conf"

  install -D "${srcdir}/${pkgname}-install.script" \
    "${pkgdir}/usr/share/libalpm/scripts/${pkgname}-install"
  install -D "${srcdir}/${pkgname}-upgrade.script" \
    "${pkgdir}/usr/share/libalpm/scripts/${pkgname}-upgrade"
  install -Dm644 "${srcdir}/${pkgname}-install.hook" \
    "${pkgdir}/usr/share/libalpm/hooks/${pkgname}-install.hook"
  install -Dm644 "${srcdir}/${pkgname}-upgrade.hook" \
    "${pkgdir}/usr/share/libalpm/hooks/${pkgname}-upgrade.hook"
}
