# Maintainer: Ian Bradley <crabapp@hikki.tech>
_pkgname=perl-app-bs
pkgname="$_pkgname-git"
_perl_namespace=App
_perl_module=BS
pkgver=0.0.1.r19.g02e58e1
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
url="https://cgit.cincotuf.lan/pennylinux/BS"
_commit=6dffadd5a1f7e75c44664445400d53cd581513c8
source=("$_pkgname::git+$url#commit=$_commit")
sha512sums=('a4d5b433cd8910e3fa871be4e0085c6d27c6b5b25128b8b6abae456b87c8797c46576c13191c98c625353cff2a16d9b8a98673b2606e0b8ad1e4161ed66e3a77')
b2sums=('9c2f8360c88a92206844ae0d1eb7ecbedf37b9e26c15700084f82866929be39354583b2bf063ecd2b6979f56f31e5dddebce490b185e50c79dfbea9e45ea860c')

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
   --with-develop \
   --with-configure"
}

build() {
  cd "$srcdir/$_pkgname"
  cpanm --installdeps .
  perl Build.PL
  ./Build build

  mkdir fat
  
  for pl in $(perl -Mv5.40 -e 'grep { open my $fh q{<}, $_; my $txt = do { local $/; $fh; }; close $fh;  $txt =~ qr$#!/usr/bin/.+perl$ } (@_)' script/*); do
    basename="$(basename "$pl")"
    fatpack pack "$pl" > "fat/$basename-fat.pl"
  done
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
