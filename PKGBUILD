# Maintainer: Carl Reinke <mindless2112 gmail com>
# Contributor: Claudio Kozicky <claudiokozicky@gmail.com>

pkgname=edge-hb
pkgver=1373004000
_pkgver=2013-07-05
pkgrel=2
pkgdesc="A casual puzzle-platformer that challenges you to roll your cube through over 100 levels. [Humble Bundle]"
url="http://twotribes.com/message/edge/"
license=('custom')
source=(hib://EDGE-Linux-"$_pkgver".sh)
md5sums=('235be99de242b6e3c62175ee28419817')
arch=('i686' 'x86_64')
depends=('openal' 'libgl')
options=('!strip' '!upx')
#PKGEXT='.pkg.tar'

package()
{
    # data
    cd "$srcdir"
    [[ -d "$pkgname"-"$pkgver" ]] && rm -r "$pkgname"-"$pkgver" # clean for `makepkg --repackage`
    sh "${source[0]#hib://}" \
        --keep \
        --nox11 \
        --target "$srcdir"/"$pkgname"-"$pkgver" \
        --unattended \
        --bindir "$pkgdir"/usr/bin \
        --datadir "$pkgdir"/opt \
        --no-register

    # launcher
    ln -s /opt/EDGE/EDGE.bin.${CARCH/i686/x86} "$pkgdir"/usr/bin/edge

    # desktop integration
    install -d "$pkgdir"/usr/share/pixmaps
    ln -s /opt/EDGE/EDGE.png "$pkgdir"/usr/share/pixmaps/edge.png
    install -Dm644 "$pkgname"-"$pkgver"/EDGE.desktop "$pkgdir"/usr/share/applications/edge.desktop
    sed -e 's/Exec=.*/Exec=edge/' \
        -e 's/Icon=.*/Icon=edge/' \
        -i "$pkgdir"/usr/share/applications/edge.desktop

    # cleanup
    cd "$pkgdir"
    rm -r opt/EDGE/xdg-utils
    rm opt/EDGE/lib$([ $CARCH = x86_64 ] && echo 64 || echo 32)/libopenal.so.1 # use system library
    rm usr/bin/uninstall-EDGE
}
