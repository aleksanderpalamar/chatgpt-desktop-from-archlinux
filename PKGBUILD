pkgname=chatgpt
pkgver=26.814.41957
pkgrel=1
pkgdesc='ChatGPT desktop application by OpenAI'
arch=('x86_64')
url='https://developers.openai.com/codex/app'
license=('custom')
depends=(
  'alsa-lib'
  'at-spi2-core'
  'cairo'
  'dbus'
  'desktop-file-utils'
  'expat'
  'gdk-pixbuf2'
  'glib2'
  'glibc'
  'gtk3'
  'libcups'
  'libdrm'
  'libgcc'
  'libglvnd'
  'libnotify'
  'libstdc++'
  'libusb'
  'libx11'
  'libxcb'
  'libxcomposite'
  'libxdamage'
  'libxext'
  'libxfixes'
  'libxkbcommon'
  'libxrandr'
  'mesa'
  'nspr'
  'nss'
  'pango'
  'systemd-libs'
  'vulkan-icd-loader'
  'xdg-utils'
)
optdepends=(
  'apparmor: apply the included ChatGPT AppArmor profile'
  'git: Git repository integration'
  'vulkan-driver: hardware-accelerated Vulkan rendering'
)
provides=('chatgpt-desktop')
conflicts=('chatgpt-desktop')
options=('!strip' '!debug' '!emptydirs')

_deb_file='chatgpt_amd64.deb'
source=("${_deb_file}")
noextract=("${_deb_file}")
sha256sums=('4778b26a7abd08647214d5b05c17bd3ebe2d9688d146dabf017c1a2faf93ac7d')

package() {
  bsdtar -xOf "${srcdir}/${_deb_file}" data.tar.xz |
    bsdtar --no-same-owner -xJf - -C "${pkgdir}"

  install -Dm644 \
    "${pkgdir}/usr/share/doc/chatgpt/copyright" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Debian-only package metadata is not useful on Arch Linux.
  rm -rf \
    "${pkgdir}/usr/share/doc" \
    "${pkgdir}/usr/share/lintian"
}
