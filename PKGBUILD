# Maintainer: tarball <bootctl@gmail.com>

pkgname=netbird-ui
pkgver=0.14.5
pkgrel=1
pkgdesc='Official GUI for the Netbird client'
url='https://netbird.io'
arch=(x86_64 aarch64 armv7h armv7l armv6h)
license=(BSD)

provides=("$pkgname")
conflicts=("$pkgname")
depends=(netbird gtk3 libayatana-appindicator)
makedepends=(go)
optdepends=()

source=(
  "$pkgname-$pkgver.tar.gz::https://github.com/netbirdio/netbird/archive/refs/tags/v$pkgver.tar.gz"
)
sha256sums=('2ea6be9c50a5ac241fbae35934c9c710697de39e8a0393f8e1800285a7904d0d')

prepare() {
  cd "$srcdir/netbird-$pkgver"
  mkdir -p build
  go mod download
}

build() {
  export GOFLAGS='-buildmode=pie -trimpath -mod=readonly -modcacherw'
  cd "$srcdir/netbird-$pkgver"

  go build \
    -ldflags "-s -w -linkmode=external -extldflags \"$LDFLAGS\"" \
    -o build/"$pkgname" \
    client/ui/client_ui.go
}

package() {
  _source="$srcdir/netbird-$pkgver"
  _ui="$_source/client/ui"

  install -d "$pkgdir/usr/bin"
  install -d "$pkgdir/usr/share/applications"
  install -d "$pkgdir/usr/share/icons"
  install -d "$pkgdir/usr/share/mime/packages"

  install -Dm755 "$_source/build/$pkgname" "$pkgdir/usr/bin/$pkgname"

  install -Dm644 "$_ui/netbird.desktop" "$pkgdir/usr/share/applications/netbird.desktop"

  install -Dm644 "$_ui/netbird.ico" "$pkgdir/usr/share/icons/netbird.ico"

  install -Dm644 "$_source/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
