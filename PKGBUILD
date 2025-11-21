# Maintainer: Ian Bradley <crabapp@hikki.tech>
_pkgname=app-bs
pkgname="$_pkgname-git"
_perl_namespace=App
_perl_module=BS
pkgver=0.0.1.r5.gb175b13
pkgrel=1
pkgdesc="Build system for PKGBUILD based Linux distributions"
arch=(any)
license=('Artistic-1.0-Perl' 'GPL-1.0-or-later')
depends=('perl' 'expac' 'pacutils' 'archlinux-contrib' 'pacman-contrib'
  'archiso' 'pacman-mirrorlist' 'shiny-mirrors-git' 'perl-net-ssleay'
  'perl-devel-checkbin' 'arch-rebuild-order' 'perl-object-pad')
makedepends=('perl-syntax-keyword-try' 'perl-cpanel-json-xs'
  'perl-app-fatpacker' 'perl-module-build' 'perl-dbi' 'cpanminus'
  'perl-inline-c' 'perl-path-tiny' 'perl-net-ssleay' 'perl-module-build-xsutil')
options=('!emptydirs' 'staticlibs')
replaces=(perl-app-bs perl-app-bs-git)
provides=(perl-app-bs-git app-bs)
conflicts=(app-bs)
url="https://cincotuf.lan/pennylinux/BS"
_commit=b175b138a07533c1a5fcf38a31f7daee756957db
source=("$_pkgname::git+$url#commit=$_commit")
sha512sums=('b9056183cab19f09cb34dd015612d0fd56f22c4da1a60dc41ea4bcf3053aa59f81807881e25ba65870ae11f6c77f085331bc2a5b4d10953913952c9b98854e33')
b2sums=('dde0f44175c1532564dc53c5916371fb5d728a392b452450b53f4369db69b8198d9a3bf0e39fc5c244e4de6b2a72653045530ce1f671d922c9f7076c8a29069b')

pkgver() {
  cd "$srcdir/$_pkgname"
  (
    set -o pipefail
    git describe --long --abbrev=7 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
      printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
  )
}

prepare() {
  cd "$srcdir/$_pkgname"

  unset PERL5LIB PERL_MM_OPT PERL_MB_OPT PERL_LOCAL_LIB_ROOT

  export PERL5LIB="$(pwd)/local/lib/perl5"
  export PERL_CPANM_OPT="-L$(pwd)/local \
   --save-dists . \
   --self-contained \
   --with-develop \
   --with-configure"
}

build() {
  cd "$srcdir/$_pkgname"
  cpanm Net::SSLeay --notest
  cpanm --installdeps .
  perl Build.PL
  ./Build build
}

check() {
  cd "$srcdir/$_pkgname"
  ./Build testall
}

package() {
  cd "$srcdir/$_pkgname"
  ./Build install \
   --destdir="$pkgdir" \
   --install_base=/usr \
   --prefix=/usr \
   --verbose
}
