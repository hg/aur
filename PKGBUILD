# Maintainer: Karol Babioch <karol@babioch.de>

pkgname=('python-cymruwhois' 'python2-cymruwhois')
_pkgname='python-cymruwhois'
pkgver=1.6
pkgrel=1
pkgdesc='Client for the whois.cymru.com service'
arch=('any')
url='https://pythonhosted.org/cymruwhois/'
license=('MIT')
makedepends=('python-setuptools' 'python2-setuptools')
source=("git+https://github.com/JustinAzoff/$_pkgname#tag=$pkgver")
sha256sums=('SKIP')

package_python-cymruwhois() {
  depends=('python')
  cd "$srcdir/$_pkgname"
  python setup.py install --root="$pkgdir/" --optimize=1
  install -Dm644 LICENSE* "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_python2-cymruwhois() {
  depends=('python2')
  cd "$srcdir/$_pkgname"
  python2 setup.py install --root="$pkgdir/" --optimize=1
  install -Dm644 LICENSE* "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

