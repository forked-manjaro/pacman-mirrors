# Maintainer: Frede Hundewadt <fh@manjaro.org>
# Contributor: Philip Müller <philm@manjaro.org>

pkgname=pacman-mirrors
pkgver=4.27
pkgrel=1
pkgdesc="Manjaro Linux mirror list for use by pacman"
arch=('any')
url="https://gitlab.manjaro.org/applications/pacman-mirrors"
license=('GPL-3.0-or-later')
depends=(
  'python'
  'python-npyscreen'
  'python-requests'
)
makedepends=(
  'curl'
  'git'
  'python-babel'
  'python-build'
  'python-poetry-core'
  'python-installer'
  'python-wheel'
)
optdepends=(
  'gtk3: for interactive mode (GUI)'
  'python-gobject: for interactive mode (GUI)'
)
provides=("pacman-mirrorlist=$pkgver-$pkgrel")
conflicts=('pacman-mirrorlist' 'reflector')
backup=("etc/$pkgname.conf")
#_commit=69a76afcb649e7dd8455ed7dd7fb28e11b485445  # tags/4.26.1^0
_tag=v4.27
source=("git+https://gitlab.manjaro.org/applications/pacman-mirrors.git#tag=${_tag}"
        "$pkgname-install.script"
        "$pkgname-upgrade.script"
        "$pkgname-install.hook"
        "$pkgname-upgrade.hook")
sha256sums=('7559575196681a5fd40df0559341dde995bc9168bfd1aca81aa0ccae083471e3'
            '718a47605be1ca328255b19047dee6d331e0440f303b86d17485fe53937b7906'
            '3b1df8c662161903653b0ae41d910019f87a58f3ecd8e02ea9ac8859b9c43f17'
            '88befb1a9b167112e05544ec4a765705bf474209e7ef67c44ffc418e10e89bfa'
            '6b6869d9dd85cd3a3cba49013dd2fc1c5f7a0934ba1284e21d4bbd24fa2540c6')

pkgver() {
  cd "$pkgname"
  git describe --tags | sed 's/^v//;s/-/+/g'
}

build() {
  cd "$pkgname"
  python -m build --wheel --no-isolation
  pybabel compile -D pacman_mirrors -d data/locale
  curl -o data/share/mirrors.json https://repo.manjaro.org/mirrors.json
}

package() {
  cd "$pkgname"
  python -m installer --destdir="$pkgdir" dist/*.whl

  install -d "$pkgdir/etc/pacman.d"
  install -Dm644 "data/etc/$pkgname.conf" -t "$pkgdir/etc/"
  install -Dm644 data/share/mirrors.json -t "$pkgdir/usr/share/$pkgname/"
  install -Dm644 "data/man/$pkgname.8.gz" -t "$pkgdir/usr/share/man/man8/"
  install -Dm644 {AUTHORS,CHANGELOG,CONTRIBUTING,README}.md -t \
    "$pkgdir/usr/share/doc/$pkgname/"

  # install locale -- there's probably a better way to do this
  pushd data/locale
  rm pacman_mirrors.pot
  for lang in *; do
    install -Dm644 "${lang}/LC_MESSAGES/pacman_mirrors.mo" -t \
      "$pkgdir/usr/share/locale/${lang}/LC_MESSAGES/"
  done
  popd

  install -Dm755 "$srcdir/$pkgname-install.script" \
    "$pkgdir/usr/share/libalpm/scripts/$pkgname-install"
  install -Dm755 "$srcdir/$pkgname-upgrade.script" \
    "$pkgdir/usr/share/libalpm/scripts/$pkgname-upgrade"
  install -Dm644 "$srcdir/$pkgname-install.hook" -t \
    "$pkgdir/usr/share/libalpm/hooks/"
  install -Dm644 "$srcdir/$pkgname-upgrade.hook" -t \
    "$pkgdir/usr/share/libalpm/hooks/"
}
