# Maintainer: Wolfgang Pupp <wolfgang.pupp@gmail.com>
# Maintainer: Martchus <martchus@gmx.net>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: namelessjon <jonathan.stott@gmail.com>
# Contributor: Alessio Sergi <asergi at archlinux dot us>
# Contributor: Alexey Pavlov <alexpux@gmail.com>

_realname=libsodium
pkgname="mingw-w64-${_realname}"
pkgver=1.0.19
pkgrel=1
pkgdesc="A modern, portable, easy to use crypto library (mingw-w64)"
arch=(any)
url="https://github.com/jedisct1/libsodium"
license=('custom:ISC')
depends=('mingw-w64-crt')
options=(!strip !buildflags staticlibs)
makedepends=('mingw-w64-configure')
source=(https://download.libsodium.org/libsodium/releases/${_realname}-${pkgver}.tar.gz{,.sig})
b2sums=('de43520150b55760142d186404cc3e49471c6e911a7a590c7ae08bc61e928c063c459555f49cd88155238fb0008ef3924b6d7c14ba9cff2f90f1e96201e1259c'
        'SKIP')

validpgpkeys=("54A2B8892CC3D6A597B92B6C210627AABA709FE1") # Frank Denis
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    mkdir -p "${srcdir}"/build-${_arch} && cd "${srcdir}"/build-${_arch}

    ${_arch}-configure ../${_realname}-stable

    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}"/build-${_arch}
    make DESTDIR="$pkgdir" install

    ${_arch}-strip --strip-unneeded "${pkgdir}"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "${pkgdir}"/usr/${_arch}/lib/*.a

    rm "${pkgdir}/usr/${_arch}/bin/"*.def
    rm -rf "${pkgdir}/usr/${_arch}/share"
  done

  install -Dm644 ${srcdir}/${_realname}-stable/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

