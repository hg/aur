# Maintainer: tarball <bootctl@gmail.com>

pkgname=netbird
pkgver=0.15.3
pkgrel=1
pkgdesc='A WireGuard-based mesh network that connects your devices into a single private network'
url='https://netbird.io'
arch=(i686 pentium4 x86_64 arm armv7h armv6h aarch64 riscv64)
license=(BSD)

provides=($pkgname)
conflicts=($pkgname)
depends=(glibc)
makedepends=('go>=1.19')
optdepends=('resolvconf: Private DNS')
replaces=(wiretrustee)

_package=github.com/netbirdio/$pkgname

source=(
  "$pkgname-$pkgver.tar.gz::https://$_package/archive/refs/tags/v$pkgver.tar.gz"
  'environment'
  'netbird@.service'
)
sha256sums=('809d1457931dff639310e1fc9a06f636cd8e71ce2798e97c26a1b1a2053d5e10'
            '128e36e1f814a12886f3122a1809a404be17f81481275b6624e66937941f5269'
            '3bd6d2692dc6d08cfabce1ba2514c02f4463294ebbdb63828baca5d9e4c9daa9')

prepare() {
  cd "$srcdir/$pkgname-$pkgver"
  mkdir -p build
  go mod download
}

build() {
  export GOFLAGS='-buildmode=pie -trimpath -mod=readonly -modcacherw'
  cd "$srcdir/$pkgname-$pkgver"

  go build \
    -ldflags "-s -w -linkmode=external -X $_package/version.version=$pkgver -extldflags \"$LDFLAGS\"" \
    -o build/"$pkgname" \
    client/main.go

  for shell in bash fish zsh; do
    ./build/"$pkgname" completion $shell >build/completion.$shell
  done
}

check() {
  cd "$srcdir/$pkgname-$pkgver/build"

  # check that version was set and it works
  [[ "$(./$pkgname version)" == $pkgver ]]
}

package() {
  _source="$srcdir/$pkgname-$pkgver"

  # binary
  install -Dm755 "$_source/build/$pkgname" "$pkgdir/usr/bin/$pkgname"

  # config directory
  install -Ddm755 -o root -g root "$pkgdir/etc/$pkgname"

  # environment file
  install -Dm644 environment "$pkgdir/etc/default/$pkgname"

  # systemd unit
  install -Dm644 "$pkgname@.service" \
    "$pkgdir/usr/lib/systemd/system/$pkgname@.service"

  # license
  install -Dm644 "$_source/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # shell completions
  install -Dm644 "$_source/build/completion.bash" \
    "$pkgdir/usr/share/bash-completion/completions/$pkgname"

  install -Dm644 "$_source/build/completion.fish" \
    "$pkgdir/usr/share/fish/completions/$pkgname.fish"

  install -Dm644 "$_source/build/completion.zsh" \
    "$pkgdir/usr/share/zsh/site-functions/_$pkgname"
}
