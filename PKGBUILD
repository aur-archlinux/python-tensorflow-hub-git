# Maintainer: Alexandr Parkhomenko <it@52tour.ru>

_author=tensorflow
_pkgname=hub
pkgname=python-tensorflow-hub-git
pkgver=0.11.0
pkgrel=1
pkgdesc='TensorFlow Hub is a repository of reusable assets for machine learning with TensorFlow. In particular, it provides pre-trained SavedModels that can be reused to solve new tasks with less training time and less training data.'
arch=(
 'any'
#  'x86_64'
)

url='https://github.com/'
license=('Apache2')
depends=('python' 'tensorflow' 'python-numpy' 'python-protobuf' 'python-six')
makedepends=('git' 'bazel')
provides=('python-tensorflow_hub' 'tensorflow_hub')
source=("git://github.com/$_author/$_pkgname")
sha256sums=('SKIP')

prepare() {
  # subs not exists
  cd "$srcdir/$_pkgname"
  git submodule init
  git submodule update
  git fetch --tags
}

pkgver () {
  cd "$srcdir/$_pkgname"
  set COMMIT=`git rev-list --count HEAD`
  git describe --tags `git rev-list --tags --max-count=1` | sed -r 's/^v//;s/-RC/RC/;s/([^-]*-g)/r\1/;s/-/./g' #| sed -r "s/0$/$COMMIT/"
}

build() {
  cd "$srcdir/$_pkgname"
  rm -rf build
  protoc -I=tensorflow_hub --python_out=tensorflow_hub tensorflow_hub/*.proto
  ls -1 tensorflow_hub/*_pb2.py
  python tensorflow_hub/pip_package/setup.py build
#  bazel build tensorflow_hub:all
}

check() {
  cd "$srcdir/$_pkgname"
#  python tensorflow_hub/pip_package/setup.py test
# see https://github.com/tensorflow/hub/blob/master/docs/build_from_source.md
##  bazel test tensorflow_hub:all
}

package() {
  cd "$srcdir/$_pkgname"
##  bazel build tensorflow_hub/pip_package:build_pip_package
##  bazel-bin/tensorflow_hub/pip_package/build_pip_package build/tensorflow_hub_pkg
##  unzip build/tensorflow_hub_pkg/*.whl
  #  develop
  python tensorflow_hub/pip_package/setup.py install --root="$pkgdir/" --skip-build --optimize=1
  install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
