# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Eric Bélanger <eric@archlinux.org>

pkgbase=webkit2gtk-4.1
pkgname=(
  webkit2gtk-4.1
  webkit2gtk-4.1-docs
)
pkgver=2.42.0
pkgrel=1
pkgdesc="Web content engine for GTK"
url="https://webkitgtk.org"
arch=(x86_64)
license=(custom)
depends=(
  at-spi2-core
  atk
  bubblewrap
  cairo
  enchant
  fontconfig
  freetype2
  glib2
  gst-plugins-bad-libs
  gst-plugins-base-libs
  gstreamer
  gtk3
  harfbuzz
  harfbuzz-icu
  hyphen
  icu
  libavif
  libdrm
  libegl
  libepoxy
  libgcrypt
  libgl
  libgles
  libjpeg
  libjxl
  libmanette
  libpng
  libseccomp
  libsecret
  libsoup3
  libsystemd
  libtasn1
  libwebp
  libwpe
  libx11
  libxcomposite
  libxml2
  libxslt
  libxt
  mesa
  openjpeg2
  sqlite
  wayland
  woff2
  wpebackend-fdo
  xdg-dbus-proxy
  zlib
)
makedepends=(
  clang
  cmake
  gi-docgen
  gobject-introspection
  gperf
  gst-plugins-bad
  lld
  ninja
  python
  ruby
  systemd
  unifdef
  wayland-protocols
)
source=(
  $url/releases/webkitgtk-$pkgver.tar.xz{,.asc}
  webkitgtk-MiniBrowser-fullscreen.patch::https://github.com/WebKit/WebKit/commit/e07345343415dd2496edc721daa61a3b42703131.patch
)
sha256sums=('828f95935861fae583fb8f2ae58cf64c63c178ae2b7c2d6f73070813ad64ed1b'
            'SKIP'
            'a921d6be1303e9f23474971f381886fd291ec5bb1a7ff1e85acede8cfb88bef2')
b2sums=('afaaef8482fe81645eee55be86a80fb51eff83dc000ac0dc5981d41810b5c72c59428d8e92a02c04718c0367ac19689501c81764f9603b767d7271ad9cd66075'
        'SKIP'
        'd440d82c769f1b35caf5464dc850cdf1c896224205c90c17d8b0a44aee62e4b1383e11306936aaca067fde8836770d346d5122d7b05c91a5c7c1741c89c65e2f')
validpgpkeys=(
  'D7FCF61CF9A2DEAB31D81BD3F3D322D0EC4582C3'  # Carlos Garcia Campos <cgarcia@igalia.com>
  '5AA3BC334FD7E3369E7C77B291C559DBE4C9123B'  # Adrián Pérez de Castro <aperez@igalia.com>
)

prepare() {
  cd webkitgtk-$pkgver

  patch -Np1 < ../webkitgtk-MiniBrowser-fullscreen.patch
}

build() {
  local cmake_options=(
    -DPORT=GTK
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_INSTALL_PREFIX=/usr
    -DCMAKE_INSTALL_LIBDIR=lib
    -DCMAKE_INSTALL_LIBEXECDIR=lib
    -DCMAKE_SKIP_RPATH=ON
    -DUSE_AVIF=ON
    -DUSE_SOUP2=OFF
    -DENABLE_DOCUMENTATION=ON
    -DENABLE_MINIBROWSER=ON
  )

  # GCC with LTO fails to link libjavascriptcoregtk
  #     /usr/bin/ld: /tmp/ccXxyWZV.ltrans0.ltrans.o: in function `ipint_table_size_validate':
  #     <artificial>:(.text+0x49f0f): undefined reference to `ipint_extern_table_size'
  #     /usr/bin/ld: /tmp/ccXxyWZV.ltrans0.ltrans.o: in function `ipint_table_fill_validate':
  #     <artificial>:(.text+0x4a019): undefined reference to `ipint_extern_table_fill'
  #     collect2: error: ld returned 1 exit status
  export CC=clang CXX=clang++
  LDFLAGS+=" -fuse-ld=lld"

  # Produce minimal debug info: 4.3 GB of debug data makes the
  # build too slow and is too much to package for debuginfod
  CFLAGS+=' -g1'
  CXXFLAGS+=' -g1'

  cmake -S webkitgtk-$pkgver -B build -G Ninja "${cmake_options[@]}"
  cmake --build build
}

package_webkit2gtk-4.1() {
  depends+=(
    libWPEBackend-fdo-1.0.so
    libwpe-1.0.so
  )
  provides+=(
    libjavascriptcoregtk-4.1.so
    libwebkit2gtk-4.1.so
  )
  optdepends=(
    'geoclue: Geolocation support'
    'gst-libav: nonfree media decoding'
    'gst-plugins-bad: media decoding'
    'gst-plugins-good: media decoding'
  )

  DESTDIR="$pkgdir" cmake --install build

  rm -r "$pkgdir/usr/bin"

  mkdir -p doc/usr/share
  mv {"$pkgdir",doc}/usr/share/gtk-doc

  cd webkitgtk-$pkgver
  find Source -name 'COPYING*' -or -name 'LICENSE*' -print0 | sort -z |
    while IFS= read -d $'\0' -r _f; do
      echo "### $_f ###"
      cat "$_f"
      echo
    done |
    install -Dm644 /dev/stdin "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_webkit2gtk-4.1-docs() {
  pkgdesc+=" (documentation)"
  depends=()

  mv doc/* "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
