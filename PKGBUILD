pkgname=nodejs
pkgver=25.0.0
pkgrel=2
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
    'git'
    'python'
    'procps-ng'
)
options=('!lto')
source=(git+ssh://git@github.com/nodejs/node.git#tag=v${pkgver})
sha256sums=(83757831f81da8011b6c732a8f61bc23f2ec4d5d41cc6caf149c91f5799a4130)

prepare() {
    cd node

    sed -i "s|lib/|lib64/|g" tools/install.py
    sed -i "s|'lib'|'lib64'|g" lib/module.js
    sed -i "s|'lib'|'lib64'|g" deps/npm/lib/npm.js
}

build() {
    cd node

    local configure_args=(
        --prefix=/usr
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

    CC="${CHOST}-gcc"
    CXX="${CHOST}-g++"

    ./configure "${configure_args[@]}"

    make
}

package() {
    cd node

    make DESTDIR=${pkgdir} install
}
