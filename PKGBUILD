pkgname=nodejs
pkgver=24.8.0
pkgrel=1
pkgdesc="Evented I/O for V8 javascript"
arch=('x86_64')
url="https://nodejs.org"
license=('MIT')
depends=(
    'brotli'
    'c-ares'
    'icu'
    'libuv'
    'nghttp2'
    'openssl'
    'simdjson'
    'which'
    'zlib'
)
makedepends=(
    'python'
    'procps-ng'
)
options=('!lto')
source=(https://nodejs.org/dist/v${pkgver}/node-v${pkgver}.tar.xz)
sha256sums=(1c03b362ebf4740d4758b9a3d3087e3de989f54823650ec80b47090ef414b2e0)

prepare() {
    cd node-v${pkgver}

    sed -i "s|lib/|lib64/|g" tools/install.py
    sed -i "s|'lib'|'lib64'|g" lib/module.js
    sed -i "s|'lib'|'lib64'|g" deps/npm/lib/npm.js
}

build() {
    cd node-v${pkgver}

    local configure_args=(
        --prefix=/usr
        --libdir=lib64
        --shared-brotli
        --shared-cares
        --shared-libuv
        --shared-openssl
        --shared-nghttp2
        --shared-openssl
        --shared-simdjson
        --shared-zlib
        --with-intl=system-icu
        --without-npm
    )

    # /usr/lib64/libnode.so uses malloc_usable_size, which is incompatible with fortification level 3
    CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
    CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

    ./configure "${configure_args[@]}"

    make -j1
}

package() {
    cd node-v${pkgver}

    make DESTDIR=${pkgdir} install
}
