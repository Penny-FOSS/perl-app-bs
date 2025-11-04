# Maintainer: Ian Bradley <crabapp@hikki.tech>
_pkgname=perl-app-bs
pkgname="$_pkgname-git"
_perl_namespace=App
_perl_module=BS
pkgver=r100.fd1fc4b
pkgrel=1
pkgdesc="Build system for PKGBUILD based Linux distributions"
arch=(any)
license=('Artistic-1.0-Perl' 'GPL-1.0-or-later')
depends=('perl' 'expac' 'pacutils' 'archlinux-contrib' 'pacman-contrib'
  'archiso' 'pacman-mirrorlist' 'shiny-mirrors-git' 'perl-net-ssleay')
makedepends=('perl-syntax-keyword-try' 'perl-cpanel-json-xs'
  'perl-app-fatpacker' 'perl-module-build' 'perl-dbi' 'cpanminus'
  'perl-inline-c' 'perl-path-tiny' 'perl-net-ssleay')
options=('!emptydirs' 'staticlibs')
url="https://cincotuf.lan/pennylinux/BS"
_tag=app-bs-0-r1
source=("git+$url#tag=$_tag?signed=1")
b2sums=('417a73e79b82d803f89a83962af950ccc5f59bbb674e824343268c73dd8154c3d207dbf6ba0437709935d9bbab88ca276c20202a95ee3930a7e2e4474bd20680')
sha513sum=('cd3937afa78831f80a2ad5abab6c51b9e82fca4c31e5856ea208d598db5dc867')
validpgpkeys=(5D8733704B740FC4B330EF5E8FFAD8B7BA73987)

pkgver() {
  cd "$srcdir/App-bs"
  (
    set -o pipefail
    git describe --long --abbrev=7 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
      printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
  )
}

prepare() {
  cd "$srcdir/App-bs"

  unset PERL5LIB PERL_MM_OPT PERL_MB_OPT PERL_LOCAL_LIB_ROOT

  export PERL5LIB="./local/lib/perl5"
  export PERL_CPANM_OPT="-L$(pwd)local \
   --verbose \
   --save-dists \
   --self-contained \
   --with-develop \
   --with-all-features"
}

build() {
  cd "$srcdir/App-bs"
  cpanm Net::SSLeay --notest
  cpanm --installdeps .
  perl Build.PL
  ./Build build
}

check() {
  cd "$srcdir/App-bs"
  ./Build testall
}

package() {
  cd "$srcdir/App-bs"
  INSTALL_BASE=/usr ./Build install --destdir "$pkgdir"
}
