# custom variables
_hkgname=data-accessor-monads-fd
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-data-accessor-monads-fd
pkgver=0.2.0.3
pkgrel=19
pkgdesc="Use Accessor to access state in monads-fd State monad class"
url="http://hackage.haskell.org/package/data-accessor-monads-fd"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc"
         "sh"
         "haskell-data-accessor"
         "haskell-monads-fd"
         "haskell-transformers")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
sha256sums=('1ff628067c081547764a92eb039ce8bfa5d79191832fcfde4beea88c4b996efb')
install="${pkgname}.install"

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
