# Maintainer: Luis Martinez <luis dot martinez at disroot dot org>

pkgname=invidtui
pkgver=0.2.2
pkgrel=1
pkgdesc="TUI-based Invidious client"
arch=('x86_64' 'i686' 'arm' 'armv6h' 'armv7h' 'aarch64')
url="https://github.com/darkhz/invidtui"
license=('MIT')
depends=('mpv' 'ffmpeg' 'youtube-dl')
makedepends=('go')
source=("$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz")
sha256sums=('c753e949f9443d28ce0526ba9d605eea3505c557b855a7a81336e95b3cca262a')

prepare() {
	cd "$pkgname-$pkgver"
	mkdir -p build
	go mod download
}

build() {
	export CGO_CPPFLAGS="${CPPFLAGS}"
	export CGO_CFLAGS="${CFLAGS}"
	export CGO_CXXFLAGS="${CXXFLAGS}"
	export CGO_LDFLAGS="${LDFLAGS}"
	export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"

	cd "$pkgname-$pkgver"
	go build -o build
}

check() {
	cd "$pkgname-$pkgver"
	go test ./...
}

package() {
	cd "$pkgname-$pkgver"
	install -D "build/$pkgname" -t "$pkgdir/usr/bin/"
	install -Dm644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname/"
	install -Dm644 README.md -t "$pkgdir/usr/share/doc/$pkgname/"
}
