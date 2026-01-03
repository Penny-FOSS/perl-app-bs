# Maintainer: Ian Bradley <crabapp@hikki.tech>
_pkgname=perl-app-bs
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
_commit=02e58e1de9fafaa7c90518a668488a470a428cc6
source=("$_pkgname::git+https://git.sr.ht/~crabapp/App-BS#commit=$_commit")
sha512sums=('c6485514060af2e83fecee35bf8a039e08c4c2b984228d8c3a7c6181ed56f1bc2bc92e79ad4bde4de1b2e96562d11d7bc4255b56219979554e5890cb4a5f51d9')
b2sums=('21676eac394bdcb0da928b22be0ac7e8e3f7545ac2da57f142a3ab8a74d095cb33bf84aac5ddf36c10573e22de9c50a0932d0fcb6989fa0b5c98dd6e46c34ff6')

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
  for pl in script/*; do
    basename="$(basename "$pl")"
    fatpack pack "$pl" >"$basename-fat.pl"
  done
}

package() {
  cd "$srcdir/$_pkgname"
  ./Build install \
    --destdir="$pkgdir" \
    --install_base=/usr \
    --prefix=/usr \
    --verbose
}
