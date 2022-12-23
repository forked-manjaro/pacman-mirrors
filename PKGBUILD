# Maintainer: Frede Hundewadt <fh@manjaro.org>
# Contributor: Philip Müller <philm@manjaro.org>

pkgname=pacman-mirrors
_date=$(date +%Y%m%d)
pkgver=4.23.2+2+g2f58b3c
pkgrel=2
pkgdesc="Manjaro Linux mirror list for use by pacman"
arch=('any')
url="https://gitlab.manjaro.org/applications/pacman-mirrors"
license=('GPL')
depends=('python' 'python-npyscreen' 'python-requests' 'python-certifi')
makedepends=('git' 'python-babel' 'python-setuptools')
optdepends=('gtk3: for interactive mode (GUI)'
            'python-gobject: for interactive mode (GUI)')
provides=("manjaro-mirrors=$pkgver-$pkgrel" "pacman-mirrorlist=$_date-$pkgrel"
          "pacman-mirrors=$pkgver-$pkgrel" 'pacman-mirrors=30850401-1')
conflicts=('pacman-mirrorlist' 'pacman-mirrors-dev' 'maint' 'reflector'
           'manjaro-mirrors')
replaces=('pacman-mirrorlist' 'manjaro-mirrors')
backup=('etc/pacman-mirrors.conf')
_commit=2f58b3c22ae481bc1467d54ea598e16f27797fa6  # master
source=("git+https://gitlab.manjaro.org/applications/pacman-mirrors.git#commit=${_commit}"
        'pacman-mirrors-install.script'
        'pacman-mirrors-upgrade.script'
        'pacman-mirrors-install.hook'
        'pacman-mirrors-upgrade.hook')
sha256sums=('SKIP'
            '718a47605be1ca328255b19047dee6d331e0440f303b86d17485fe53937b7906'
            '3b1df8c662161903653b0ae41d910019f87a58f3ecd8e02ea9ac8859b9c43f17'
            '88befb1a9b167112e05544ec4a765705bf474209e7ef67c44ffc418e10e89bfa'
            '6b6869d9dd85cd3a3cba49013dd2fc1c5f7a0934ba1284e21d4bbd24fa2540c6')

pkgver() {
  cd "${srcdir}/pacman-mirrors"
  git describe --tags | sed 's/^v//;s/-/+/g'
}

prepare() {
  cd "${srcdir}/pacman-mirrors"
}

build() {
  cd "${srcdir}/pacman-mirrors"
  python setup.py compile_catalog --directory locale --domain pacman_mirrors
}

package() {
  cd "${srcdir}/pacman-mirrors"
   python setup.py install --root="$pkgdir" --optimize=1

  install -Dm644 "${srcdir}/pacman-mirrors-install.script" "${pkgdir}/usr/share/libalpm/scripts/pacman-mirrors-install"
  install -Dm644 "${srcdir}/pacman-mirrors-upgrade.script" "${pkgdir}/usr/share/libalpm/scripts/pacman-mirrors-upgrade"
  install -Dm644 "${srcdir}/pacman-mirrors-install.hook" "${pkgdir}/usr/share/libalpm/hooks/pacman-mirrors-install.hook"
  install -Dm644 "${srcdir}/pacman-mirrors-upgrade.hook" "${pkgdir}/usr/share/libalpm/hooks/pacman-mirrors-upgrade.hook"
}
