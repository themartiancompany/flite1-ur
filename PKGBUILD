# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer : Daniel Bermond <dbermond@archlinux.org>
# Contributor: der_FeniX <derfenix@gmail.com>
# Contributor: Albert Graef <aggraef@gmail.com>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
fi
_pkg="flite"
pkgname="${_pkg}1"
pkgver=1.4
pkgrel=7
pkgdesc='A lighweight speech synthesis engine (version 1.x)'
arch=(
  'x86_64'
  'arm'
  'armv7l'
  'aarch64'
  'armv6l'
  'mips'
  'pentium4'
  'powerpc'
)
url="http://www.speech.cs.cmu.edu/${_pkg}"
license=(
  'custom'
)
depends=(
  "${_libc}"
)
makedepends=(
  'texlive-plaingeneric'
  'ed'
)
provides=(
  "${_pkg}=${pkgver}"
  "${pkgname}-patched"
)
conflicts=(
  "${_pkg}"
  "${pkgname}-patched"
)
replaces=(
  "${pkgname}-patched"
)
_tarname="${_pkg}-${pkgver}"
_patches=(
  "010-${pkgname}-tempfile-CVE-2014-0027.patch"
  "020-${pkgname}-fix-parallel-builds.patch"
  "030-${pkgname}-respect-destdir.patch"
  "040-${pkgname}-ldflags.patch"
  "050-${pkgname}-audio-interface.patch"
  "060-${pkgname}-texi.patch"
  "070-${pkgname}-texi2html-to-texi2any-migration.patch"
  "080-${pkgname}-no-rpath.patch"
  "090-${pkgname}-rename-conflicting-variable.patch"
)
source=(
  "http://www.festvox.org/${_pkg}/packed/${_tarname}/${_tarname}-release.tar.bz2"
  "${_patches[@]}"
)
sha256sums=(
  '45c662160aeca6560589f78daf42ab62c6111dd4d244afc28118c4e6f553cd0c'
  '597f1516060917faab008819e3ceb5bb487f5b3948e97eef1020dc10b62c6edf'
  'bfd51888ea533bb9ee74cadb68b2e507cb715ab5043aa679b7f42ab52336a7a1'
  '093538c3a7cd2b9b9edd1f0956a34c4261c3ccdd4feb55e8ecedc338562495f3'
  'ff43e11241c9aea26483865c672c20421d12c688ae8b59b39471bafb52c1463e'
  '405320984e098c3d788b7751935b2774972ee7970dbe0fef0718ce1e5cc725c9'
  'd38fa5dfd4fef71970d904622ec106b9ac18ece002c671b14bc1ce9b342b56b6'
  '1b51d528e3927b80159c6f6c2155fc022f807db7a0cf19c50e9a5e5831086efb'
  '462b9ecdb3e4992cb2fc026b6483ec83d883ece530a3fa0794a00e4f6fbfbb1a'
  '9ad072d57d7b3d6a623f4885cf90a6548d6c5091cd00a7c0c8ff317f4fc0f7f1'
)

prepare() {
  local \
    _patch
  for _patch in "${_patches[@]}"; do
    patch \
      -d \
        "${_tarname}-release" \
    -Np1 \
    -i \
    "${srcdir}/${_patch}"
  done
}

build() {
  cd \
    "${_tarname}-release"
  CFLAGS+=' -Wno-incompatible-pointer-types' \
  ./configure \
    --prefix='/usr' \
    --enable-shared \
    --disable-static \
    --with-vox='cmu_us_kal16'
  make \
    -j1
  make \
    -C \
      doc \
    "${_pkg}.pdf"
}

package() {
  make \
    -C \
      "${_tarname}-release" \
    DESTDIR="${pkgdir}" \
    install
  install \
    -Dm644 \
    "${_tarname}-release/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install \
    -Dm644 \
    "${_tarname}-release/doc/${_pkg}.pdf" \
    -t \
    "${pkgdir}/usr/share/doc/${pkgname}"
}
