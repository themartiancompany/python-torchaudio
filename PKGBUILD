# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Jingbei Li <i@jingbei.li>
# Contributer: Jose Riha <jose1711 gmail com>

_cuda="false"
_py="python"
_Pkg=audio
_pkg="torch${_Pkg}"
pkgname="${_py}-${_pkg}"
pkgver=2.5.1
_sox_ver=14.4.2
pkgrel=1
pkgdesc=(
  "Data manipulation and transformation"
  "for audio signal processing, powered by PyTorch"
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
  'i686'
  'arm'
  'aarch64'
  'armv7l'
  'armv6l'
  'mips'
  'pentium4'
  'powerpc'
)
_http="https://github.com"
_ns="pytorch"
url="${_http}/${_ns}/${_Pkg}"
license=(
  'BSD'
)
depends=(
  "${_py}"
  "${_py}-pytorch"
  'bzip2'
  'xz'
  'opencore-amr'
  'lame'
  'libogg'
  'libFLAC.so'
  'libvorbis'
  'opus'
  'opusfile'
  'zlib'
)
optdepends=(
  "${_py}-kaldi-io"
)
if [[ "${_cuda}" == "true" ]]; then
  optdepends+=(
    'cuda'
  )
fi
makedepends=(
  'git'
  "${_py}-setuptools"
  'cmake'
  'ninja'
  'boost'
)
conflicts=(
  "${_py}-torchaudio-git"
)
source=(
  "${url}/archive/refs/tags/v${pkgver}.tar.gz"
  "https://downloads.sourceforge.net/project/sox/sox/${_sox_ver}/sox-${_sox_ver}.tar.bz2"
  "7797f83e1d66ff78872763e1da3a5fb2f0534c40.patch"
)
sha256sums=(
  '200fbb1234c104a3662b444c0bec2acf4049c4b2113a73c0dc5f4e672cc2a4cc'
  '81a6956d4330e75b5827316e44ae381e6f1e8928003c6aa45896da9041ea149c'
  '9d1d6c5018e08cedef4028b23418f7ad69a2d24065509a0697bad25532fd9484'
)

prepare() {
  ln \
    -s \
    "${srcdir}/sox-${_sox_ver}" \
    "${srcdir}/${_Pkg}-${pkgver}/third_party/sox"
  cd \
    "${srcdir}/${_Pkg}-${pkgver}"
  patch \
    -p1 < \
    "${srcdir}/7797f83e1d66ff78872763e1da3a5fb2f0534c40.patch"
}

build() {
  cd \
    "$srcdir/${_Pkg}-${pkgver}"
  if [[ "${_cuda}" == "true" ]]; then
    export \
      CUDACXX="/opt/cuda/bin/nvcc"
    export \
      CUDAHOSTCXX=$CXX
    export \
      TORCH_CUDA_ARCH_LIST="5.2;5.3;6.0;6.1;6.2;7.0;7.2;7.5;8.0;8.6;8.9;9.0;9.0+PTX"
  fi
  # Follow architectures used by pytorch
  # https://github.com/archlinux/svntogit-community/blob/packages/python-pytorch/trunk/PKGBUILD
 
  if [[ "${_cuda}" == "true" ]]; then
    CUDA_HOME=/opt/cuda/ \
    BUILD_SOX=1 \
    "${_py}" \
      setup.py \
      build
  elif [[ "${_cuda}" == "false" ]]; then
    BUILD_SOX=1 \
    "${_py}" \
      setup.py \
      build
  fi 
}

package() {
  cd \
    "$srcdir/${_Pkg}-${pkgver}"
  BUILD_SOX=1 \
  "${_py}" \
    setup.py \
      install \
      --root="${pkgdir}" \
      --optimize=1
  install \
    -Dm644 \
    "LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
