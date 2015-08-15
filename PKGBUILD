# Maintainer : Lone_Wolf lonewolf@xs4all.nl
# Contributor : Robert McCathie <archaur at rmcc dot com dot au>
# Contributor: Juraj Misur <juraj.misur@gmail.com>
# Contributor: Tom < reztho at archlinux dot us >

pkgname=cndrvcups-capt
pkgver=2.56
pkgrel=1
pkgdesc="Canon CAPT Printer Driver for Linux"
arch=('x86_64')
url="http://support-au.canon.com.au/contents/AU/EN/0900772408.html"
license=('custom')
install=cndrvcups-capt.install
_MachineType=`uname -m`
if [[ ${_MachineType} == i686 ]]; then
  echo "Package is x86_64 only for now, aborting makepkg"
  return 1
else
  depends=('lib32-libxml2' 'lib32-libcups' 'lib32-gcc-libs' 'cndrvcups-common=2.56' 'lib32-popt')
fi

makedepends=('autoconf' 'automake')
optdepends=('gtk2: for gui')
options=(!strip)
source=('http://gdlp01.c-wss.com/gds/6/0100004596/02/Linux_CAPT_PrinterDriver_V256_uk_EN.tar.gz'
	'ccpd.service'
	'how-to.txt')
backup=(etc/ccpd.conf)

md5sums=('3c5c1265d74088695d81808ba8672799'
         '481f47f14de57a5dfe951da3ab4e331c'
         '5bf11334e3eda599e44da451bdbdc363')

build() {
    cd ${srcdir}/Linux_CAPT_PrinterDriver_V256_uk_EN/Src
    tar xf ${pkgname}-${pkgver}-1.tar.gz
    #prepare build directory
    rm -rf ${pkgname}-build
    cp -r "${pkgname}-${pkgver}-1" "${srcdir}/${pkgname}-build"
    cd "${srcdir}/${pkgname}-build"
    cd driver 
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr --libdir=/usr/lib --enable-progpath=/usr/bin --disable-static
    cd ../backend 
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr --libdir=/usr/lib --enable-progpath=/usr/bin
    cd ../pstocapt 
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr --libdir=/usr/lib --enable-progpath=/usr/bin
    cd ../pstocapt2 
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr --libdir=/usr/lib --enable-progpath=/usr/bin
    cd ../pstocapt3 
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr --libdir=/usr/lib --enable-progpath=/usr/bin
    cd ../ppd 
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr 
    cd ../statusui
    sed -i 's@#include <cups/cups.h>@#include <cups/cups.h>\n#include <cups/ppd.h>@' "src/ppapdata.c"
    sed -i 's@char req_header\[4\];@char req_header\[5\];@' "cnsktmodule/cnsktmodule.h"
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    sed -i 's/AM_PROG_CC_STDC/#AM_PROG_CC_STDC/' configure.in
    ./autogen.sh --prefix=/usr --libdir=/usr/lib
    sed -i 's@captstatusui_LDADD = -lgtk-x11-2.0@captstatusui_LDADD = -lpthread -lgtk-x11-2.0@' "src/Makefile"
    cd ../cngplp
    libtoolize --force --copy
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr --libdir=/usr/lib
    cd files
    sed -i 's/AM_CONFIG_HEADER/#AM_CONFIG_HEADER/' configure.in
    ./autogen.sh --prefix=/usr
    cd "${srcdir}/${pkgname}-build"
    make
}

package() {
    mkdir -p ${pkgdir}/etc
    mkdir -p ${pkgdir}/usr/share/{captfilter,ccpd,captmon,captmon2,captemon,cups/model,caepcm,cngplp}
    mkdir -p ${pkgdir}/usr/lib32
    mkdir -p ${pkgdir}/usr/lib/systemd/system/
    cd ${srcdir}/${pkgname}-build
    make install DESTDIR=${pkgdir}
# adapted from cndrvcups-capt.spec
    install -s -m 755 libs/captmon/captmon         ${pkgdir}/usr/bin
    install -s -m 755 libs/captmon2/captmon2       ${pkgdir}/usr/bin
    install -s -m 755 libs/captfilter              ${pkgdir}/usr/bin
    install    -m 755 libs/ccpddata/CNAB1CL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNAC4CL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNAC5CL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNAC6CL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNAB7CL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/cnab6cl.bin    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNAC8CL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNAC9CL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNAC9CLS.BIN   ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNABBCL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNABBCLS.BIN   ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNACACL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/ccpddata/CNACBCL.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/captmon/msgtable.xml    ${pkgdir}/usr/share/captmon
    install    -m 755 libs/captmon2/msgtable2.xml  ${pkgdir}/usr/share/captmon2
    install -s -m 755 libs/ccpd         ${pkgdir}/usr/bin
    install -s -m 755 libs/ccpdadmin    ${pkgdir}/usr/bin
    install    -m 755 samples/ccpd.conf ${pkgdir}/etc
    install -s -m 755 libs/captemon/captmonlbp5000       ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmonlbp3300       ${pkgdir}/usr/bin
    install    -m 755 libs/captemon/msgtablelbp5000.xml  ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablelbp3300.xml  ${pkgdir}/usr/share/captemon
    install -s -m 755 libs/captemon/captmoncnac5         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnac6         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnac8         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnac9         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnab6         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnab7         ${pkgdir}/usr/bin
    install    -m 755 libs/captemon/msgtablecnac5.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnac6.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnab6.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnab7.xml    ${pkgdir}/usr/share/captemon
    install -s -m 755 libs/captemon/captmoncnab8         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnab9         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnaba         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnabb         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnabc         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnabd         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnaca         ${pkgdir}/usr/bin
    install -s -m 755 libs/captemon/captmoncnacb         ${pkgdir}/usr/bin
    install    -m 755 libs/captemon/msgtablecnab8.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnab9.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnaba.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnac8.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnac9.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnabb.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnabc.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnabd.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnaca.xml    ${pkgdir}/usr/share/captemon
    install    -m 755 libs/captemon/msgtablecnacb.xml    ${pkgdir}/usr/share/captemon
    install    -m 644 data/CNL8* ${pkgdir}/usr/share/caepcm
    install    -m 644 data/CNL7* ${pkgdir}/usr/share/caepcm
    install    -m 644 data/CNL9* ${pkgdir}/usr/share/caepcm
    install    -m 644 data/CNLA* ${pkgdir}/usr/share/caepcm
    install    -m 644 data/CNLC* ${pkgdir}/usr/share/caepcm
    install    -m 644 data/CNLE* ${pkgdir}/usr/share/caepcm
    install    -m 644 data/CnA*  ${pkgdir}/usr/share/caepcm
    install -s -m 755 libs/libcaptfilter.so.1.0.0  ${pkgdir}/usr/lib32
    install    -m 755 libs/libcaiocaptnet.so.1.0.0 ${pkgdir}/usr/lib32
    install    -m 755 libs/libcncaptnpm.so.2.0.1   ${pkgdir}/usr/lib32
    install    -m 755 libs/captdrv     ${pkgdir}/usr/bin
    install    -m 755 libs/CnAC*INK.DAT      ${pkgdir}/usr/share/captfilter
    install    -m 755 libs/captemon/CNAC*.BIN    ${pkgdir}/usr/share/ccpd
    install    -m 755 libs/libcnaccm.so.1.0    ${pkgdir}/usr/lib32
    cd ${pkgdir}/usr/lib32
    ln -sf libcaiocaptnet.so.1.0.0  libcaiocaptnet.so.1
    ln -sf libcaiocaptnet.so.1.0.0  libcaiocaptnet.so
    ln -sf libcncaptnpm.so.2.0.1  libcncaptnpm.so.2
    ln -sf libcncaptnpm.so.2.0.1  libcncaptnpm.so
    ln -sf libcaptfilter.so.1.0.0 libcaptfilter.so.1
    ln -sf libcaptfilter.so.1.0.0 libcaptfilter.so
    ln -sf libcnaccm.so.1.0   libcnaccm.so.1
    ln -sf libcnaccm.so.1.0   libcnaccm.so
    cd ${srcdir}/${pkgname}-build
      
    install -m755 "${srcdir}/ccpd.service" "${pkgdir}/usr/lib/systemd/system"
    mkdir -p "${pkgdir}/var/ccpd"
    mkdir -p "${pkgdir}/var/captmon"
    install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
    install -m644 LICENSE-capt-${pkgver}* "${pkgdir}/usr/share/licenses/${pkgname}/"
}
