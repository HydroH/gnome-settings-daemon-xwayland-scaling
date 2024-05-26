# Maintainer: @HydroH <https://github.com/hydroh>

# Archlinux credits:
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>

# Patch credits:
# https://gitlab.gnome.org/GNOME/gnome-settings-daemon/-/merge_requests/353/
# Author: Jonas Ådahl <jadahl@gmail.com>

pkgname=gnome-settings-daemon-xwayland-scaling
pkgver=46.0
pkgrel=2
pkgdesc="GNOME Settings Daemon with Xwayland fractional scaling patch"
url="https://gitlab.gnome.org/GNOME/gnome-settings-daemon"
arch=(x86_64)
license=(
  GPL-2.0-or-later
  LGPL-2.0-or-later
)
depends=(
  alsa-lib
  bash
  cairo
  dconf
  fontconfig
  gcc-libs
  gcr-4
  geoclue
  geocode-glib-2
  glib2
  glibc
  gnome-desktop
  gsettings-desktop-schemas
  gtk3
  libcanberra-pulse
  libcolord
  libcups
  libgudev
  libgweather-4
  libmm-glib
  libnm
  libnotify
  libp11-kit
  libpulse
  librsvg
  libwacom
  libx11
  libxext
  libxfixes
  libxi
  nss
  pango
  polkit
  pulse-native-provider
  systemd
  systemd-libs
  upower
  wayland
)
makedepends=(
  docbook-xsl
  git
  libxslt
  meson
  python
  python-packaging
  usbguard
)
provides=(gnome-settings-daemon=$pkgver)
conflicts=(gnome-settings-daemon)
optdepends=('usbguard: USB protection support')
groups=(gnome)
backup=(etc/xdg/Xwayland-session.d/00-xrdb)
_commit=8ff4196096efba8b2441ee31a63c8bd8cea4b1d8  # tags/46.0^0
source=(
  "git+https://gitlab.gnome.org/GNOME/gnome-settings-daemon.git#commit=$_commit"
  "git+https://gitlab.gnome.org/GNOME/libgnome-volume-control.git"
  0001-subprojects-Update-gvc-to-latest-commit.patch
  0001-xsettings-Get-UI-scaling-factor-from-dedicated-D-Bus.patch
)
b2sums=('d3e0c207fa2df397a9f2d0c39c68d8fbc719f1962915130e10641bf2ca765e86b05b5d512789c65d1641aad5f9986cb0bb0c21c12f36d288201cdb89c7790e73'
        'SKIP'
        '51cfe280b938ae8c74a46432feddbafb598d7e82fec7dfbf657791cb4749a0a205d5e99decb4953272451b03c91fe7c3891df0c4e945c2070615405db3ec897c'
        '4802ee3c5fe903d51980897f714e9a639b59a8e4ff7861f9767855cb34ecf0a7a0bdf3a00300a2e2af8eb6b04bfa7b900571007531df402c3e6f6c264eefcd78')

pkgver() {
  cd gnome-settings-daemon
  git describe --tags | sed -r 's/\.([a-z])/\1/;s/([a-z])\./\1/;s/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd gnome-settings-daemon

  git apply -3 ../0001-subprojects-Update-gvc-to-latest-commit.patch

  git submodule init
  git submodule set-url subprojects/gvc "$srcdir/libgnome-volume-control"
  git -c protocol.file.allow=always submodule update

  patch -p1 -N -i "${srcdir}/0001-xsettings-Get-UI-scaling-factor-from-dedicated-D-Bus.patch"
}

build() {
  arch-meson gnome-settings-daemon build
  meson compile -C build
}

package() {
  meson install -C build --destdir "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
