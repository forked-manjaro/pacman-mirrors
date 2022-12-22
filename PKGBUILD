# Maintainer: Frede Hundewadt <fh@manjaro.org>
# Contributor: Philip Müller <philm@manjaro.org>

pkgname=pacman-mirrors
pkgver=4.23.2+2+g2f58b3c
pkgrel=1
pkgdesc="Manjaro Linux mirror list for use by pacman"
arch=('any')
url="https://gitlab.manjaro.org/applications/pacman-mirrors"
license=('GPL')
depends=('python' 'python-npyscreen' 'python-requests' 'python-certifi')
makedepends=('git' 'python-babel' 'python-setuptools')
optdepends=('gtk3: for interactive mode (GUI)'
            'python-gobject: for interactive mode (GUI)')
conflicts=('pacman-mirrorlist' 'reflector')
backup=('etc/${pkgname}.conf')
_commit=2f58b3c22ae481bc1467d54ea598e16f27797fa6  # master
source=("git+https://gitlab.manjaro.org/applications/pacman-mirrors.git#commit=${_commit}"
        "${pkgname}-install.script"
        "${pkgname}-upgrade.script"
        "${pkgname}-install.hook"
        "${pkgname}-upgrade.hook")
sha256sums=('SKIP'
            '718a47605be1ca328255b19047dee6d331e0440f303b86d17485fe53937b7906'
            '3b1df8c662161903653b0ae41d910019f87a58f3ecd8e02ea9ac8859b9c43f17'
            '88befb1a9b167112e05544ec4a765705bf474209e7ef67c44ffc418e10e89bfa'
            '6b6869d9dd85cd3a3cba49013dd2fc1c5f7a0934ba1284e21d4bbd24fa2540c6')

pkgver() {
  cd "${srcdir}/${pkgname}"
  git describe --tags | sed 's/^v//;s/-/+/g'
}

prepare() {
  cd "${srcdir}/${pkgname}"
}

build() {
  cd "${srcdir}/${pkgname}"
  python setup.py build
  python setup.py compile_catalog --directory locale --domain pacman_mirrors
}

package() {
  cd "${srcdir}/${pkgname}"
   python setup.py install --root="$pkgdir" --optimize=1 --skip-build

  install -Dm644 "${srcdir}/${pkgname}-install.script" "${pkgdir}/usr/share/libalpm/scripts/${pkgname}-install"
  install -Dm644 "${srcdir}/${pkgname}-upgrade.script" "${pkgdir}/usr/share/libalpm/scripts/${pkgname}-upgrade"
  install -Dm644 "${srcdir}/${pkgname}-install.hook" "${pkgdir}/usr/share/libalpm/hooks/${pkgname}-install.hook"
  install -Dm644 "${srcdir}/${pkgname}-upgrade.hook" "${pkgdir}/usr/share/libalpm/hooks/${pkgname}-upgrade.hook"
}
