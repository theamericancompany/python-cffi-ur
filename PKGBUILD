# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: lilydjwg <lilydjwg@gmail.com>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=cffi
pkgname="${_py}-${_pkg}"
pkgver=1.17.1
pkgrel=2
_pkgdesc=(
  "Foreign Function Interface"
  "for Python calling C code"
)
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'mips'
  'i686'
  'powerpc'
  'pentium4'
)
url="https://${_pkg}.readthedocs.org"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-pycparser"
)
optdepends=(
  "${_py}-setuptools: 'limited api' version checking in ${_pkg}.setuptools_ext"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest"
)
_http="https://github.com"
_ns="${pkgname}"
_url="${_http}/${_ns}/${_pkg}"
_tarname="${_pkg}-${pkgver}"
source=(
  "${_url}/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz"
)
sha512sums=(
  'bb22f2f21f4d9e097bdacaad24b883936304e794d0e319f24db794de37e47de690b3c352487d670e3b9e2322d5144cd3d3582fb847c4f6806be5eb549e63d9de'
)

build() {
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      build \
    -nw
}

check() {
  local \
    _python_version
  _python_version=$( \
    "${_py}" \
      -c \
        'import sys; print(".".join(map(str, sys.version_info[:2])))')
  cd \
    "${_tarname}"
  "${_py}" \
    -m \
      installer \
    --destdir=tmpinstall \
    "dist/"*".whl"
  PYTHONPATH="${PWD}/tmpinstall/usr/lib/python${_python_version}/site-packages" \
  pytest
}

package() {
  cd \
    "${_tarname}"
  # remove files created during check()
  # for reproducible SOURCES.txt
  rm \
    -rf \
    "testing/cffi"{0,1}"/__pycache__"
  "${_py}" \
    -m installer \
    --destdir="${pkgdir}" \
    "dist/"*".whl"
  install \
    -Dm644 \
    "LICENSE" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}
