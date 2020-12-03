# Maintainer: Sefa Eyeoglu <contact@scrumplex.net>

_pkgname=espanso
pkgname=${_pkgname}
pkgver=0.7.2
pkgrel=3
pkgdesc="Cross-platform Text Expander written in Rust"
arch=(x86_64)
url="https://espanso.org/"
license=("GPL3")
depends=("xdotool" "xclip" "libxtst" "libnotify")
optdepends=("modulo: Support for interactive forms")
makedepends=("rust" "git" "cmake")
install="${pkgname}.install"
source=("${_pkgname}::git+https://github.com/federico-terzi/espanso.git#tag=v${pkgver}")
sha512sums=('SKIP')


prepare() {
    cd "$_pkgname"

    # don't change the original service file, as it will be embedded in the binary
    cp "src/res/linux/systemd.service" "systemd.service"
    sed -i "s|{{{espanso_path}}}|/usr/bin/espanso|g" "systemd.service"

    cargo update -p serde --precise 1.0.117
    cargo update -p serde_yaml --precise 0.8.14
}

check() {
    cd "$_pkgname"

    cargo test --release --locked
}

build() {
    cd "$_pkgname"

    cargo build --release --locked
}

package() {
    cd "$_pkgname"

    install -Dm755 "target/release/${_pkgname}" "${pkgdir}/usr/bin/${_pkgname}"
    install -Dm644 "systemd.service" "${pkgdir}/usr/lib/systemd/user/${_pkgname}.service" # install our own copy

    install -Dm644 "README.md" "${pkgdir}/usr/share/doc/${_pkgname}/README.md"
}
