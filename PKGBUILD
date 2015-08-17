# Contributor: JokerBoy <jokerboy at punctweb dot ro>
# Contributor: Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-bfs-beta
pkgver=295.17
_extramodules=extramodules-3.2-bfs
_kernver="$(cat /lib/modules/${_extramodules}/version)"
pkgrel=2
pkgdesc="NVIDIA beta drivers for linux-bfs."
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux-bfs>=3.2' 'linux-bfs<3.3' "nvidia-utils-beta=${pkgver}")
makedepends=('linux-bfs-headers>=3.1' 'linux-bfs-headers<3.2')
conflicts=('nvidia-bfs-96xx' 'nvidia-bfs-173xx' 'nvidia-bfs')
provides=("nvidia=${pkgver}" "nvidia-bfs=${pkgver}")
license=('custom')
install=nvidia.install
options=(!strip)
[ "$CARCH" = "i686" ] && _arch='x86' && _pkg="NVIDIA-Linux-${_arch}-${pkgver}" && md5sums=('453d7585254cb2abb486135828278e43')
[ "$CARCH" = "x86_64" ] && _arch='x86_64' && _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32" && md5sums=('784480587bb000df46065a853e370ef0')
source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")

build() {
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}/kernel"
    make SYSSRC="/lib/modules/${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/lib/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/lib/modprobe.d/nvidia_bfs.conf"
    echo "blacklist nvidiafb" >> "${pkgdir}/lib/modprobe.d/nvidia_bfs.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/lib/modules/${_extramodules}/nvidia.ko"
}
