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
optdepends=('raine-emudx: improved graphic and sound files for some classic games')
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

  sed -i 's/#define RDTSC/#undef RDTSC/' source/profile.h
  sed -i 's/#define DEF_INLINE inline/#define DEF_INLINE static/' source/raine.h
  sed -i 's/#define DEF_INLINE __inline__/#define DEF_INLINE static/' source/raine.h
  sed -i 's/define UINT64 __int64/define UINT64 uint64_t/' source/profile.h
  sed -i 's|extern DATAFILE \*RaineData;|// extern DATAFILE *RaineData;|g' source/raine.h
  sed -i 's|#include "gui.h"|#include "sdl2/gui.h"|g' source/raine.h
  sed -i 's|#include "rgui.h"|#include "sdl2/dialogs/messagebox.h"|g' source/raine.h
  sed -i 's/^RAINE_DEBUG = 1/#RAINE_DEBUG = 1/' makefile
}

build() {
  cd "$srcdir/raine-$pkgver"
  EXTRA_INCLUDES="-Isource/sdl2 -Isource/sdl2/dialogs $(find source -type d -exec printf "-I%s " {} +)"
  make NO_ASM=1 CFLAGS+="-Wno-implicit-function-declaration -fcommon -fgnu89-inline -DSDL=2 -I/usr/include/SDL2 $(sdl2-config --cflags) ${EXTRA_INCLUDES}"
}

package() {
  cd "$srcdir"/raine-$pkgver
  make NO_ASM=1 DESTDIR="$pkgdir" install
  # doc + license
  install -d "$pkgdir"/usr/share/{doc,licenses}/raine
  install -m644 docs/* changes/* "$pkgdir"/usr/share/doc/raine
  head -n5 source/raine.c > "$pkgdir"/usr/share/licenses/raine/LICENSE
}
