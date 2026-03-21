# Maintainer: carstene1ns <arch carsten-teibes de> - http://git.io/ctPKG
# Contributor: trya <tryagainprod@gmail.com>
# Contributor: robb_force <robb_force@holybuffalo.net>

pkgname=raine64
pkgver=0.97.5d
pkgrel=1
pkgdesc="A multiple arcade emulator focused on 680x0/z80 machines like
NeoCD and Neo Geo, C version"
url="http://raine.1emulation.com/"
license=('custom')
arch=('x86_64')
replaces=('neoraine','raine64')
depends_x86_64=('sdl2' 'sdl2_ttf' 'sdl2_image' 'curl' 'openssl' 'lua')
makedepends=('mesa')
optdepends=(
            'raine-emudx: improved graphic and sound files for some classic games')
source=(raine-$pkgver.tar.gz::"https://github.com/zelurker/raine/archive/$pkgver.tar.gz")
sha256sums=('d1ee4dd556058e7ca77b474c065f9c7c1889cefe0d2e02ea39c512931ef2b1dd')
options=('emptydirs')

prepare() {
  cd "$srcdir"
  mkdir -p raine-bin

  cd raine-$pkgver

  # adapt folder structure to arch standards
  sed 's|$(prefix)/games|\$(prefix)/bin|;s|$(prefix)/share/games|\$(prefix)/share|' -i makefile
  sed 's|share/games/raine|share/raine|' -i source/sdl/dialogs/about.cpp source/raine.c
  sed -i -e 's|#include "debug.h"|#include "../../debug.h"|g' -e 's|#include "dasm.h"|#include "../../dasm.h"|g' source/mame/z80/z80dasm.c
  sed -i -e 's|#include "deftypes.h"|#include "../../deftypes.h"|g' -e 's|#include "mame/pair.h"|#include "../pair.h"|g' source/mame/z80/z80.h
  sed -i -e 's|#include "debug.h"|#include "../../debug.h"|g' -e 's|#include "dasm.h"|#include "../../dasm.h"|g' source/mame/6502/6502dasm.c
  sed -i -e 's|#include "deftypes.h"|#include "../../deftypes.h"|g' -e 's|#include "mame/pair.h"|#include "../pair.h"|g' -e 's|#include "mame/handlers.h"|#include "../handlers.h"|g' source/mame/6502/m6502.h
  sed -i 's|#include "debug.h"|#include "../debug.h"|g' source/mame/handlers.h
  sed -i 's/^RAINE_DEBUG = 1/#RAINE_DEBUG = 1/' makefile
}

build() {
  make NO_ASM=1 -C "$srcdir"/raine-$pkgver CFLAGS+="-Wno-implicit-function-declaration" #VERBOSE=1
}

package() {
  cd "$srcdir"/raine-$pkgver
  make NO_ASM=1 DESTDIR="$pkgdir" install
  # doc + license
  install -d "$pkgdir"/usr/share/{doc,licenses}/raine
  install -m644 docs/* changes/* "$pkgdir"/usr/share/doc/raine
  head -n5 source/raine.c > "$pkgdir"/usr/share/licenses/raine/LICENSE
}
