pkgname=zfs-utils
pkgver=0.8.0.rc4+b689de85
pkgrel=1
pkgdesc="Userspace utilities for the Zettabyte File System."
arch=("i686" "x86_64")
url="https://zfsonlinux.org/"
license=('CDDL')
makedepends=("git")
source=("git+https://github.com/zfsonlinux/zfs.git#commit=b689de85e8c78b9dc03c1bd4c3ca0f4e75714e92"
        "zfs.initcpio.install"
        "zfs.initcpio.hook")
sha256sums=('SKIP'
            '11e02412055fe3daf4753cf2ce7011e95647bb4c9ae74288a2a769f6a4c091b2'
            'f95ad1a5421ccbb8b01f448373f46cfd1f718361a82c2687a597325cf9827e3e')

prepare() {
    cd "${srcdir}"/zfs

    autoreconf -fi
}

build() {
    cd "${srcdir}"/zfs

    ./configure --prefix=/usr \
                --sysconfdir=/etc \
                --sbindir=/usr/bin \
                --with-mounthelperdir=/usr/bin \
                --libdir=/usr/lib \
                --datadir=/usr/share \
                --includedir=/usr/include \
                --with-udevdir=/usr/lib/udev \
                --libexecdir=/usr/lib/zfs \
                --with-config=user
    make -j $(( $(nproc) + 1))
}

package() {
    cd "${srcdir}"/zfs

    make DESTDIR="${pkgdir}" install
    install -D -m644 contrib/bash_completion.d/zfs "${pkgdir}"/usr/share/bash-completion/completions/zfs

    # Remove uneeded files
    rm -r "${pkgdir}"/etc/init.d
    rm -r "${pkgdir}"/etc/sudoers.d #???
    rm -r "${pkgdir}"/usr/lib/dracut
    rm -r "${pkgdir}"/usr/lib/modules-load.d
    rm -r "${pkgdir}"/usr/share/initramfs-tools
    rm -r "${pkgdir}"/usr/share/zfs

    install -D -m644 "${srcdir}"/zfs.initcpio.hook "${pkgdir}"/usr/lib/initcpio/hooks/zfs
    install -D -m644 "${srcdir}"/zfs.initcpio.install "${pkgdir}"/usr/lib/initcpio/install/zfs
}
