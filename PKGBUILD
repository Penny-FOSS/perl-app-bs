# Maintainer: Ian Bradley <crabapp@hikki.tech>
_pkgname=perl-app-bs
pkgname="$_pkgname-git"
_perl_namespace=App
_perl_module=BS
pkgver=app.bs.0.r1.r3.gb47116d
pkgrel=1
pkgdesc="Build system for PKGBUILD based Linux distributions"
arch=(any)
license=('Artistic-1.0-Perl' 'GPL-1.0-or-later')
depends=('perl' 'expac' 'pacutils' 'archlinux-contrib' 'pacman-contrib'
  'archiso' 'pacman-mirrorlist' 'shiny-mirrors-git' 'perl-net-ssleay'
  'perl-devel-checkbin')
makedepends=('perl-syntax-keyword-try' 'perl-cpanel-json-xs'
  'perl-app-fatpacker' 'perl-module-build' 'perl-dbi' 'cpanminus'
  'perl-inline-c' 'perl-path-tiny' 'perl-net-ssleay' 'perl-module-build-xsutil')
options=('!emptydirs' 'staticlibs')
url="https://cincotuf.lan/pennylinux/BS"
_commit=b47116d5eb7ea7242d1e669496aa569784de5504
source=("$_pkgname::git+$url#commit=$_commit")
sha512sums=('bcde089675abc5a2cc51af183405b4d8084f8fccb9e892ac3bce13b3fb299f804944e15d56ace8125e186b43cee99c95881b537f4571908ab3809eddef7aad42')
b2sums=('b946a755cddf7463ee75fb2c07f68e54eb313ea15267973a5b80ba55b025089c4e8db3b948c914908fa52aacb211217230d2d39dff38b540fb1299db253d373d')

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
   --with-configure \
   --verify \
   --with-all-features"
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
  INSTALL_BASE=/usr ./Build install --destdir "$pkgdir"
}
