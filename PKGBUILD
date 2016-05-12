# Maintainer: James An <james@jamesan.ca>
# Contributor: Sven Schneider <archlinux.sandmann@googlemail.com>

pkgname=ompl-git
_pkgname=${pkgname%-git}
pkgver=1.0.0.r402.gc1bceb6
pkgrel=1
pkgdesc="The Open Motion Planning Library (OMPL) consists of many state-of-the-art sampling-based motion planning algorithms"
arch=('i686' 'x86_64')
url="http://ompl.kavrakilab.org"
license=('BSD')
depends=('boost-libs' 'python' 'python-matplotlib')
makedepends=('boost' 'cmake' 'git')
optdepends=('py++: Python binding'
            'ode: Plan using the Open Dynamics Engine'
            'eigen: For an informed sampling technique')
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
source=("$_pkgname"::"git+https://github.com/$_pkgname/$_pkgname.git")
md5sums=('SKIP' 'SKIP')

pkgver() {
    cd "$_pkgname"
    (
        set -o pipefail
        git describe --long --tag | sed -r 's/([^-]*-g)/r\1/;s/-/./g' ||
        printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
    )
}

prepare() {
  cd "$_pkgname"

  sed -i '/^\s*#include <boost/random/mersenne_twister.hpp>\s*$/a #include <boost/type_traits/ice.hpp>' src/ompl/util/RandomNumbers.h

  # (Re)create empty build folder
  [ -d build ] && rm -rf build &>/dev/null
  mkdir build
}

build() {
  cd "$_pkgname/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DPYTHON_EXEC=/usr/bin/python2 \
    -DCMAKE_CXX_FLAGS=-D_POSIX_VERSION \
    -DOMPL_REGISTRATION=Off ..
  make
}

check() {
  cd "$_pkgname/build"

  make test
}


package() {
  cd "$_pkgname"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  cd build
  make DESTDIR="$pkgdir" install
}

