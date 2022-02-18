# Maintainer: Yuntian Zhang < yt at radxa dot com >
pkgname='dtbocfg-dkms'
_pkgname="${pkgname%-*}"
pkgver=0.0.9
pkgrel=1
pkgdesc='Device Tree Blob Overlay Configuration File System'
url='https://github.com/ikwzm/dtbocfg'
license=('BSD')
arch=('x86_64' 'i686' 'aarch64')
depends=('dkms' 'ruby')
source=("https://github.com/ikwzm/dtbocfg/archive/refs/tags/v$pkgver.tar.gz"
        "dkms.conf")
md5sums=('45037fde41c25fc4e55f7ece81b04a74'
         '35ef0e40e40ffd760c4ec02b894216f9')

prepare() {
  echo "$_pkgname" >| "${_pkgname}.conf"
}

package() {
    installDir="$pkgdir/usr/src/$_pkgname-$pkgver"

    # install dkms.conf
    install -dm755 "$installDir"
    install -m644 "$srcdir/dkms.conf" "$installDir"
    sed -e "s/@_PKGNAME@/${_pkgname}/" -e "s/@PKGVER@/${pkgver}/" -i "${installDir}/dkms.conf"

    # load kernel module on boot
    install -dm755 "$pkgdir/etc/modules-load.d"
    install -m644 "${_pkgname}.conf" "$pkgdir/etc/modules-load.d"

    # install source files
    cd $srcdir/$_pkgname-$pkgver
    for f in $(find . -type f); do
        install -m644 "$f" "$installDir/$f"
    done

    # install helper script
    install -dm755 "$pkgdir/usr/bin"
    install -m755 "dtbocfg.rb" "$pkgdir/usr/bin/dtbocfg"
    # patch location for configfs
    sed -e "s|/config/device-tree/overlays/|/sys/kernel/config/device-tree/overlays/|" -i "$pkgdir/usr/bin/dtbocfg"
}
