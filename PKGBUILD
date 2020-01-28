# Based on the file created for Manjaro Linux by:
# Maintainer: Philip Müller <philm@manjaro.org>
# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Helmut Stult <helmut@manjaro.org>

# Based on the file created for Arch Linux by:
# Maintainer : Thomas Baechler <thomas@archlinux.org>

# Maintainer: Emanuele Ballarin (Clearer Manjaro x86_64) <emanuele@ballarin.cc>
# And many other contributors (patches, suggestions, development, testing, ...)

_linuxprefix=linux55-CLEARER
_extramodules=extramodules-5.5-CLEARER
pkgname=$_linuxprefix-nvidia-440xx
_pkgname=nvidia
pkgver=440.44
pkgrel=1
_CLEARERrel=12
pkgdesc="NVIDIA drivers for linux. Clearer Manjaro kernel."
arch=('x86_64')
url="http://www.nvidia.com/"
depends=("$_linuxprefix" "nvidia-440xx-utils=${pkgver}")
makedepends=("$_linuxprefix-headers")
groups=("$_linuxprefix-extramodules")
replaces=("$_linuxprefix-$_pkgname")
provides=("$_pkgname=$pkgver")
conflicts=("$_linuxprefix-nvidia-340xx" "$_linuxprefix-nvidia-390xx" "$_linuxprefix-nvidia-418xx" "$_linuxprefix-nvidia-430xx" "$_linuxprefix-nvidia-435xx" "nvidia-340xx" "nvidia-390xx" "nvidia-418xx" "nvidia-430xx" "nvidia-435xx")
license=('custom')
install=nvidia-CLEARER.install
options=(!strip)
durl="http://us.download.nvidia.com/XFree86/Linux-x86"
source=("kernel-5.5.patch")
sha256sums=('b46333bf617044088bb5223c25fef6c01a885ea08a095df5473e5257d3082e39')
source_x86_64=("${durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run"
                "nvidia-performance-trailing.patch")
sha256sums_x86_64=('794fdfc8e65c203ae482f59df7e55050ddcf0a11af2a95eaa1a10c7d48ec7e0f'
                  'SKIP')

[[ "$CARCH" = "x86_64" ]] && _pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}"
    # patches here
    # Fix compile problem with 5.5
    (patch -p1 --no-backup-if-mismatch -i "$srcdir"/kernel-5.5.patch)

    patch -Np1 -i ../nvidia-performance-trailing.patch

}

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${_pkg}"/kernel
    make -j12 SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-modeset.ko" \
         "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-modeset.ko"
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-drm.ko" \
         "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-drm.ko"
    if [[ "$CARCH" = "x86_64" ]]; then
        install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-uvm.ko" \
            "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-uvm.ko"
    fi
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/"*.ko
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia-CLEARER.install"
}
