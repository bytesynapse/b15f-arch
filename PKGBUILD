# Maintainer: Bytesynapse <bytesynapse@proton.me>
pkgname='b15f-git'
pkgver=r242.5a49a37
pkgrel=1
pkgdesc="The library and software for interacting with the B15F board."
arch=('x86_64')
url="https://github.com/devfix/b15f"
license=('MIT')
depends=('avr-libc' 'avrdude' 'ncurses' 'gcc' 'astyle')
makedepends=('git' 'make')
install='b15f.install'
source=(
    '50-b15f.rules'
    'b15f.mcu'
    'b15f-git::git+https://github.com/devfix/b15f.git'
)
sha256sums=('SKIP' 'SKIP' 'SKIP')

pkgver() {
    cd "$srcdir/$pkgname"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
    cd "$srcdir/$pkgname"

    # read desired MCU from shipped b15f.mcu
    # mcu=$(<"$srcdir/b15f.mcu")

    # Compile firmware (optional; uncomment if needed)
    # cd "$srcdir/$pkgname/firmware"
    # Patch firmware/Makefile in-place
    # sed -i "s@\\s*MCU\\s*=\\s*atmega1284.*@MCU        = ${mcu} # set by /etc/b15f.mcu@g" "firmware/"?akefile
    # cd firmware
    # make clean
    # make

    # Compile control software
    cd "$srcdir/$pkgname/control/src"
    make clean
    make CXXFLAGS="-I/usr/avr/include"
}

package() {
    cd "$srcdir/$pkgname"

    # cd "$srcdir/$pkgname/firmware"
    # echo "Uploading firmware to microcontroller..."
    # make upload || {
    #   echo "Error: Upload failed. Is B15 powered on?"
    #   exit 1
    # }

    # Install udev rule
    install -Dm644 "$srcdir/50-b15f.rules" \
        "$pkgdir/etc/udev/rules.d/50-b15f.rules"

    # Install cli components
    cd "$srcdir/$pkgname/control/bin"
    install -Dm755 b15fcli "$pkgdir/usr/bin/b15fcli"
    install -Dm755 plotty "$pkgdir/usr/bin/plotty"
    cd "$srcdir/$pkgname/control/lib"
    install -Dm755 libb15fdrv.so "$pkgdir/usr/bin/libb15fdrv.so"

    # Install shared resources
    install -d "$pkgdir/usr/share/$pkgname"
}
