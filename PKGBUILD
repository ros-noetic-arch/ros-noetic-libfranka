pkgdesc="C++ library for Franka Emika research robots "
url="http://wiki.ros.org/libfranka"

pkgname="ros-noetic-libfranka"
pkgver="0.8.0"
pkgrel=1
pkgdesc="C++ library for Franka Emika research robots "
arch=('i686' 'x86_64' 'aarch64' 'armv7h' 'armv6h')
url="http://wiki.ros.org/libfranka"
license=('Apache')

ros_makedepends=()
makedepends=('cmake' 'git' ${ros_makedepends[@]})

ros_depends=()
depends=('eigen' 'poco' ${ros_depends[@]})

source=(
    "git+https://github.com/frankaemika/libfranka-common.git"
    "git+https://github.com/frankaemika/libfranka.git#tag=$pkgver"
    "fix_missing_includes.patch"
)

sha256sums=(
    SKIP
    SKIP
    SKIP
)

prepare() {
    # add submodule
    cd ${srcdir}/libfranka
    git submodule init
    git config submodule.common.url $srcdir/libfranka-common
    git submodule update

    # fix missing string include
    cd ${srcdir}/libfranka
    patch --forward --strip=1 --input="${srcdir}/fix_missing_includes.patch"
}

build() {
	# Create build directory
  	[ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  	cd ${srcdir}/build

	# Build project
	cmake ${srcdir}/libfranka \
          -DCATKIN_BUILD_BINARY_PACKAGE=ON \
	      -DCMAKE_INSTALL_PREFIX=/opt/ros/noetic \
          -DPYTHON_EXECUTABLE=/usr/bin/python \
          -DSETUPTOOLS_DEB_LAYOUT=OFF
	make
}

package() {
	cd "${srcdir}/build"
	make DESTDIR="${pkgdir}/" install
}
