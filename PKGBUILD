# Maintainer: Jack Sorrell <jack@jacksorrell.com>

# Uses patches from https://github.com/minus7/redshift and applies to https://github.com/jonls/redshift master

pkgname="redshift-wayland-patch"
pkgver="1.12"
pkgrel=1
pkgdesc="Adjusts the color temperature of your screen according to your surroundings with Wayland support."
arch=('i686' 'x86_64')
license=('GPL3')
provides=('redshift' 'redshift-wayland-git')
conflicts=('redshift' 'redshift-wayland-git')
depends=('geoclue2' 'libdrm' 'libxcb' 'libxxf86vm')
optdepends=('python-gobject: for redshift-gtk'
            'python-xdg: for redshift-gtk'
            'librsvg: for redshift-gtk'
            'weston: supporting wayland environment'
            'sway-git: supporting wayland environment'
            'orbital-git: supporting wayland environment')
makedepends=('intltool' 'python')
source=(redshift::"git+https://github.com/jonls/redshift.git"
		'wayland-support.patch')
sha256sums=('SKIP'
			'875dd9be0d79236015257c90db68209b4a29111c98634cdf155e37f64344ad69')

_configure_options=(
	--prefix="/usr"
	--enable-drm
	--enable-randr
	--enable-vidmode
	--enable-geoclue2
	--enable-wayland
	--with-systemduserunitdir="/usr/lib/systemd/user"
)
			
prepare() {
	cd "${srcdir}/redshift"
    git checkout "release-${pkgver}"
	
	msg "Patch to support wayland"
    patch -Np1 -i "${srcdir}/wayland-support.patch"
    
    msg "Bootstrapping"
	./bootstrap
}

build() {
    cd "${srcdir}/redshift"

    msg "Run ./configure"
    ./configure \
		"${_configure_options[@]}"
        
	msg "Run make"
    make
}

package() {
    cd "${srcdir}/redshift"
    
    msg "Package redshift"
    make DESTDIR="${pkgdir}" install
}
