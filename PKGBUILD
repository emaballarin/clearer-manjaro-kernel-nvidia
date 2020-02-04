# Based on the file created for Arch Linux by:
# Maintainer : Thomas Baechler <thomas@archlinux.org>

# Maintainer: Philip Müller <philm@manjaro.org>
# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Helmut Stult <helmut@manjaro.org>

_linuxprefix=linux55
_extramodules=extramodules-5.5-MANJARO
pkgname=$_linuxprefix-nvidia-440xx
_pkgname=nvidia
pkgver=440.59
pkgrel=1
pkgdesc="NVIDIA drivers for linux."
arch=('x86_64')
url="http://www.nvidia.com/"
depends=("$_linuxprefix" "nvidia-440xx-utils=${pkgver}")
makedepends=("$_linuxprefix-headers")
groups=("$_linuxprefix-extramodules")
provides=("$_pkgname=$pkgver")
replaces=("$_linuxprefix-$_pkgname")
conflicts=('nvidia-340xx' 'nvidia-390xx' 'nvidia-418xx' 'nvidia-430xx' 'nvidia-435xx')
license=('custom')
install=nvidia.install
options=(!strip)
durl="http://us.download.nvidia.com/XFree86/Linux-x86"
source_x86_64=("${durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
sha256sums_x86_64=('87c05aeadd260034b4d429de5be8032ead2103adee7aedc1adbb15c0ce3c6f28')

[[ "$CARCH" = "x86_64" ]] && _pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}"
    # patches here

}

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${_pkg}"/kernel
    make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
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
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
}
