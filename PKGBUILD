# Maintainer: Frede Hundewadt <fh@manjaro.org>
# Contributor: Philip Müller <philm@manjaro.org>

# test package branch
_branch=master
#_branch=v4.21.x-stable
_date=$(date +%Y%m%d)
pkgname=pacman-mirrors
_pkgname=pacman-mirrors
pkgver=4.23.2
pkgrel=1
pkgdesc="Manjaro Linux mirror list for use by pacman"
arch=('any')
depends=('python' 'python-npyscreen' 'python-requests' 'python-certifi')
makedepends=('git' 'python-babel' 'python-setuptools')
optdepends=('gtk3: for interactive mode (GUI)'
            'python-gobject: for interactive mode (GUI)')
# test package source
#url="https://codeberg.org/fhdk/pacman-mirrors"
url="https://gitlab.manjaro.org/applications/pacman-mirrors"
conflicts=('pacman-mirrorlist' 'pacman-mirrors-dev' 'maint' 'reflector')
replaces=('pacman-mirrorlist')
provides=("pacman-mirrorlist=$_date" "pacman-mirrors=$pkgver")
license=('GPL')
backup=('etc/pacman-mirrors.conf')
source=("git+$url.git#branch=$_branch"
        'pacman-mirrors-install.script'
        'pacman-mirrors-upgrade.script'
        'pacman-mirrors-install.hook'
        'pacman-mirrors-upgrade.hook')
sha256sums=('SKIP'
            '718a47605be1ca328255b19047dee6d331e0440f303b86d17485fe53937b7906'
            '3b1df8c662161903653b0ae41d910019f87a58f3ecd8e02ea9ac8859b9c43f17'
            '88befb1a9b167112e05544ec4a765705bf474209e7ef67c44ffc418e10e89bfa'
            '6b6869d9dd85cd3a3cba49013dd2fc1c5f7a0934ba1284e21d4bbd24fa2540c6')

prepare() {
  cd "${srcdir}"/$_pkgname
  # do something here
}

package() {
  cd "${srcdir}"/$_pkgname
  # make DESTDIR="${pkgdir}" install
  make clean
  make mo-files
  python setup.py install --root=${pkgdir} --optimize=1

  install -D ${srcdir}/pacman-mirrors-install.script ${pkgdir}/usr/share/libalpm/scripts/pacman-mirrors-install
  install -D ${srcdir}/pacman-mirrors-upgrade.script ${pkgdir}/usr/share/libalpm/scripts/pacman-mirrors-upgrade
  install -Dm644 ${srcdir}/pacman-mirrors-install.hook ${pkgdir}/usr/share/libalpm/hooks/pacman-mirrors-install.hook
  install -Dm644 ${srcdir}/pacman-mirrors-upgrade.hook ${pkgdir}/usr/share/libalpm/hooks/pacman-mirrors-upgrade.hook
}
