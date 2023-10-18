# Maintainer: Frede Hundewadt <fh@manjaro.org>
# Contributor: Philip Müller <philm@manjaro.org>

pkgname=pacman-mirrors-dev
pkgver=5.0.1
pkgrel=2
pkgdesc="Manjaro Linux mirror list for use by pacman"
arch=('any')
url="https://gitlab.manjaro.org/applications/pacman-mirrors"
license=('GPL3')
depends=('python' 'python-npyscreen' 'python-requests')
makedepends=('git' 'python-babel' 'python-build' 'python-poetry-core'
             'python-installer' 'python-wheel' 'python-toml')
checkdepends=('xdg-utils')
optdepends=('gtk3: for interactive mode (GUI)'
            'python-gobject: for interactive mode (GUI)')
provides=('pacman-mirrors' 'pacman-mirrorlist')
conflicts=('pacman-mirrors' 'pacman-mirrorlist' 'reflector')
backup=('etc/pacman-mirrors.conf')
_commit=1db436d70bb53a04b744a087d253799a9230b7ab  # branch/mirror-manager
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
  cd pacman-mirrors
  python -c 'import toml; print(toml.load("pyproject.toml")["tool"]["poetry"]["version"])'
}

build() {
  cd pacman-mirrors
  python -m build --wheel --no-isolation
  pybabel compile -D pacman_mirrors -d data/locale
}

package() {
  cd pacman-mirrors
  python -m installer --destdir="$pkgdir" dist/*.whl

  mkdir -p "$pkgdir/etc/pacman.d"
  install -Dm644 data/etc/pacman-mirrors.conf -t "$pkgdir/etc/"
  install -Dm644 data/share/status.json -t "$pkgdir/var/lib/$pkgname/"
  install -Dm644 data/man/pacman-mirrors.8.gz -t "$pkgdir/usr/share/man/man8/"
  install -Dm644 {AUTHORS,CHANGELOG,CONTRIBUTING,README}.md -t \
    "$pkgdir/usr/share/docs/pacman-mirrors/"

  # install locale -- there's probably a better way to do this
  pushd data/locale
  rm pacman_mirrors.pot
  for lang in *; do
    install -Dm644 "${lang}/LC_MESSAGES/pacman_mirrors.mo" -t \
      "$pkgdir/usr/share/locale/${lang}/LC_MESSAGES/"
  done
  popd

  install -Dm755 "$srcdir/pacman-mirrors-install.script" \
    "$pkgdir/usr/share/libalpm/scripts/pacman-mirrors-install"
  install -Dm755 "$srcdir/pacman-mirrors-upgrade.script" \
    "$pkgdir/usr/share/libalpm/scripts/pacman-mirrors-upgrade"
  install -Dm644 "$srcdir/pacman-mirrors-install.hook" -t \
    "$pkgdir/usr/share/libalpm/hooks/"
  install -Dm644 "$srcdir/pacman-mirrors-upgrade.hook" -t \
    "$pkgdir/usr/share/libalpm/hooks/"
}
