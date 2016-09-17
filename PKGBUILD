# $Id$
# Maintainer: Malah <malah@neuf.fr>
# Contributor: Daniel Isenmann <daniel@archlinux.org>
# Contributor: arojas
# Contributor: Timm Preetz <timm@preetz.us>
# Contributor: Giovanni Scafora <giovanni@archlinux.org>

pkgname=monodevelop
pkgver=6.1.0.5441
pkgrel=2
pkgdesc="An IDE primarily designed for C# and other .NET languages"
arch=('any')
url="http://www.monodevelop.com"
license=('GPL')
depends=('mono>=4.0.1' 'mono-addins>=0.6.2' 'gnome-sharp' 'hicolor-icon-theme')
makedepends=('rsync' 'cmake' 'git' 'nuget' 'mono-pcl' 'http-parser' 'curl')
options=(!makeflags)
optdepends=('xsp: To run ASP.NET pages directly from monodevelop')
source=("git://github.com/mono/monodevelop.git#tag=${pkgname}-$pkgver"
"e4037243e8ba2d78136d033578efd9b48a5a3fa3.patch")
md5sums=('SKIP'
         'f1650a4327743719e15799c2ca88310a')

prepare() {
  cd $srcdir/$pkgname
  patch -Np1 -i $srcdir/e4037243e8ba2d78136d033578efd9b48a5a3fa3.patch
}

build() {
  export MONO_SHARED_DIR=$srcdir/src/.wabi
  mkdir -p $MONO_SHARED_DIR

  cd $srcdir/$pkgname
  git submodule update --init --recursive || return 1
  git checkout tags/$pkgname-$pkgver
  git clean -dfx

  ./configure --prefix=/usr --profile=stable
  XDG_CONFIG_HOME="$srcdir"/config LD_PRELOAD="" make
}

package() {
  cd $srcdir/$pkgname

  XDG_CONFIG_HOME="$srcdir"/config LD_PRELOAD="" make DESTDIR=$pkgdir install
  # delete conflicting files
  rm -r $(find $pkgdir/usr/share/mime/ -type f | grep -v "packages")
  rm -r $MONO_SHARED_DIR

  # NuGet.exe is missing somehow, fixed FS#43423
  install -Dm755 "${srcdir}"/monodevelop/main/external/nuget-binary/nuget.exe "${pkgdir}"/usr/lib/monodevelop/AddIns/MonoDevelop.PackageManagement/NuGet.exe
}
