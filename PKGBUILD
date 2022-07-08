# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <blacksky3@tuta.io> <https://github.com/blacksky3>


#Upstream pages
#https://repo.radeon.com/amdgpu

major=22.20
minor=1438747
ubuntu_ver=22.04

pkgbase=ocl-icd-amdgpu-pro
pkgname=(ocl-icd-amdgpu-pro lib32-ocl-icd-amdgpu-pro)
pkgver=${major}_${minor}
pkgrel=1
arch=(x86_64)
url='https://repo.radeon.com/amdgpu'
license=(custom: multiple)
makedepends=(wget)
source=(https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/ocl-icd-amdgpu-pro/ocl-icd-libopencl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/ocl-icd-amdgpu-pro/ocl-icd-libopencl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/ocl-icd-amdgpu-pro/ocl-icd-libopencl1-amdgpu-pro-dev_${major}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/ocl-icd-amdgpu-pro/ocl-icd-libopencl1-amdgpu-pro-dev_${major}-${minor}~${ubuntu_ver}_i386.deb)

# extracts a debian package
# $1: deb file to extract
extract_deb(){
  local tmpdir="$(basename "${1%.deb}")"
  rm -Rf "$tmpdir"
  mkdir "$tmpdir"
  cd "$tmpdir"
  ar x "$1"
  tar -C "${pkgdir}" -xf data.tar.xz
}

# move ubuntu specific /usr/lib/x86_64-linux-gnu to /usr/lib
# $1: debian package library dir (goes from opt/amdgpu or opt/amdgpu-pro and from x86_64 or i386)
# $2: arch package library dir (goes to usr/lib or usr/lib32)
move_libdir(){
  local deb_libdir="$1"
  local arch_libdir="$2"

  if [ -d "${pkgdir}/${deb_libdir}" ]; then
    if [ ! -d "${pkgdir}/${arch_libdir}" ]; then
      mkdir -p "${pkgdir}/${arch_libdir}"
    fi
    mv -t "${pkgdir}/${arch_libdir}/" "${pkgdir}/${deb_libdir}"/*
    find ${pkgdir} -type d -empty -delete
  fi
}

# move copyright file to proper place and remove debian changelog
move_copyright(){
  find ${pkgdir}/usr/share/doc -name "changelog.Debian.gz" -delete
  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  find ${pkgdir}/usr/share/doc -name "copyright" -exec mv {} ${pkgdir}/usr/share/licenses/${pkgname} \;
  find ${pkgdir}/usr/share/doc -type d -empty -delete
}

package_ocl-icd-amdgpu-pro(){
  pkgdesc='AMD OpenCL ICD Loader library'
  arch=(x86_64)
  depends=(glibc)
  optdepends=('opencl-driver: packaged opencl driver')
  conflicts=(libcl ocl-icd ocl-icd-git khronos-ocl-icd-git ocl-icd-amdgpu-pro-21.20)
  replaces=(libcl)
  provides=(opencl-icd-loader ocl-icd ocl-icd-amdgpu-pro)

  extract_deb "${srcdir}"/ocl-icd-libopencl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/ocl-icd-libopencl1-amdgpu-pro-dev_${major}-${minor}~${ubuntu_ver}_amd64.deb
  move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib"
  move_copyright

  sed -i "s#prefix=/opt/amdgpu-pro#prefix=/usr#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
  sed -i "s#libdir=\${prefix}/lib/x86_64-linux-gnu#libdir=\${prefix}/lib#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
  sed -i "s#L/opt/amdgpu-pro/lib/x86_64-linux-gnu#L/usr/lib#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
  sed -i "s#-I/opt/amdgpu-pro/include#-I/usr/include#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
}

package_lib32-ocl-icd-amdgpu-pro(){
  pkgdesc='AMD OpenCL ICD Loader library (32-bit)'
  arch=(i686 x86_64)
  depends=(lib32-glibc ocl-icd-amdgpu-pro=${major}_${minor}-${pkgrel})
  optdepends=('lib32-opencl-driver: packaged opencl driver')
  conflicts=(lib32-libcl lib32-ocl-icd lib32-ocl-icd-git lib32-khronos-ocl-icd-git lib32-ocl-icd-amdgpu-pro-21.20)
  replaces=(lib32-libcl)
  provides=(lib32-opencl-icd-loader lib32-ocl-icd lib32-ocl-icd-amdgpu-pro)

  extract_deb "${srcdir}"/ocl-icd-libopencl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/ocl-icd-libopencl1-amdgpu-pro-dev_${major}-${minor}~${ubuntu_ver}_i386.deb
  move_libdir "opt/amdgpu-pro/lib/i386-linux-gnu" "usr/lib32"
  move_copyright

  sed -i "s#prefix=/opt/amdgpu-pro#prefix=/usr#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
  sed -i "s#libdir=\${prefix}/lib/x86_64-linux-gnu#libdir=\${prefix}/lib32#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
  sed -i "s#L/opt/amdgpu-pro/lib/x86_64-linux-gnu#L/usr/lib32#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
  sed -i "s#-I/opt/amdgpu-pro/include#-I/usr/include#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
}

sha256sums=('1f2fc0129594b8cae7a2692735ac768889eef472e19fb6a55035caeeb9ff97db'
            '8fbfe7c3c446ee9864dc492ae6fc68128817046a72b30b8220a45b865890f18f'
            'cbfddfce012ef922024289a5e84a44bbf20cc26c0c04d501cc54f13c9a553c2e'
            '7857150fa764cd0b476cf0da4d1e8b52b66f34a4c48a4939c9751bba849a695d')
