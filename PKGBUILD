# Maintainer: Redderick Shohart <s.sovetnik@gmail.com>

pkgname=qudos-zws
pkgver=0.40.3.3
pkgrel=1
pkgdesc="Enhanced Quake 2 engine, forked by Zws, with yamagi's fixes, and some minor patches to prevent libjpeg and zlib compilling errors"
arch=(i686)
url="http://forum.ubuntu.ru/index.php?topic=146763.150"
license=('GPL')
# Needs sdl to compile.
depends=('libjpeg' 'libpng' 'libvorbis' 'libxxf86vm' 'mesa' 'sdl')
optdepends=('libpng-old: in case of libpng errors')
makedepends=('xf86vidmodeproto')
provides=('qudos')
conflicts=('qudos')
install=qudos.install
source=(https://dl.dropbox.com/u/43220776/ZwS-qudos-4485ff0.tar.gz
        qudos.desktop
	https://dl.dropbox.com/u/43220776/Q2.png
	qudos.install
	https://dl.dropbox.com/u/43220776/QuDos-0.40.1.pk3
	https://dl.dropbox.com/u/43220776/QuDosmaX-0.40.1.tar.bz2
	https://dl.dropbox.com/u/43220776/gl_rmisc.patch)
        md5sums=('da38c6b0c327a3d4157437ebfca85fdc'
         'b8be459dbd697855dfc8294c1c6a032b'
	'e454a9fa13ca81cf155a65f6089a6b20'
	'1c040c8589ce24a0abe8453f37dcf4b5'
	'e21a824f08a653f116a3c336fd340cee'
	'43872123620e605638cc011dee03ea48'
	'a2fa34a9986cf1fae0cc4a42de88f2f1')

_gamedir="/usr/share/quake2"
_libdir="/usr/lib/qudos"
_basedir="/usr/share/quake2"

build() {
	cd $startdir/src/ZwS-qudos-4485ff0
  	patch -i $startdir/gl_rmisc.patch $startdir/src/ZwS-qudos-4485ff0/src/ref_gl/gl_rmisc.c

  msg "Starting build"
 

  # Favours OpenGL over SDL - it does not lose focus when audacious/xmms starts.
  # XMMS is disabled because the Makefile blindly assumes it is installed.
  # OSS works on x86_64.
  make \
    DATADIR=$_gamedir \
    LIBDIR=$_libdir \
    LOCALBASE=/usr \
    BUILD_GLX=YES \
    BUILD_SDLGL=YES \
    BUILD_OSS_SND=YES \
    BUILD_ALSA_SND=YES \
    BUILD_QUAKE2=YES \
    BUILD_GAME=YES \
    WITH_DATADIR=YES \
    WITH_LIBDIR=YES \
    WITH_XMMS=NO \
    X11BASE=/usr/X11\
|| return 1

  mkdir -p $startdir/pkg/$_libdir
  cp -r $startdir/src/quake2/* $startdir/pkg/$_libdir || return 1
  install -D -m755 qudos/QuDos $startdir/pkg/usr/bin/qudos || return 1
  install -D -m644 $startdir/QuDos-0.40.1.pk3 $startdir/pkg/$_basedir/baseq2/qudos.pk3 || return 1

  # Desktop entry
  install -D -m644 $startdir/Q2.png $startdir/pkg/usr/share/pixmaps/qudos.png || return 1
  install -D -m644 $startdir/src/qudos.desktop $startdir/pkg/usr/share/applications/qudos.desktop || return 1

  # Docs
  mkdir -p $startdir/pkg/usr/share/doc/qudos
  cp docs/{QuDos,Ogg_readme,todo}.txt $startdir/pkg/usr/share/doc/qudos/ || return 1
}
