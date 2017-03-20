project (word-article-template)
cmake_minimum_required (VERSION 2.8)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

set(BOOK_NAME ango)
set(LATEXMK_FLAGS "")

set(TEX_FILES intro.tex secret.tex public.tex hash.tex elliptic.tex group.tex pairing.tex homo.tex abe.tex broadcast.tex assumption.tex field_math.tex ec_math.tex search.tex zero.tex pairing_math.tex rand.tex appendix.tex proxy.tex fe.tex)
set(_TEX_FILES _intro.tex _secret.tex _public.tex _hash.tex _elliptic.tex _group.tex _pairing.tex _homo.tex _abe.tex _broadcast.tex _assumption.tex _field_math.tex _ec_math.tex _search.tex _zero.tex _pairing_math.tex _rand.tex _appendix.tex _proxy.tex _fe.tex)
set(PDF_IMAGES img/dh.pdf img/dh-ok.pdf img/dh-ng.pdf img/elgamal-weak.pdf
	img/circle.pdf img/dq3.pdf img/dq3-2.pdf
	img/elliptic.pdf img/pairing.pdf img/timecapsule.pdf
	img/kusuri.pdf img/tversky.pdf img/ss.pdf img/extract.pdf
	img/access-tree.pdf img/no-pfs.pdf img/pfs.pdf
	img/birthday.pdf img/dh-oracle.pdf img/cbc.pdf img/cbc-dec.pdf
	img/combi.pdf img/area.pdf img/ec-add.pdf img/p1.pdf img/rho.pdf
	img/mac.pdf img/iterated-hash.pdf img/bad-hmac.pdf img/protocol.pdf
	img/broad1.pdf img/broad-tree.pdf img/broad-revocation.pdf
	img/easy-traitor.pdf img/traitor.pdf img/bdhe.pdf img/pairing2.pdf
	img/distortion.pdf img/pairing-def.pdf img/mov.pdf img/mov2.pdf
	img/peks1.pdf img/peks2.pdf img/vote.pdf img/blind.pdf img/psi.pdf
	img/stream.pdf img/tokei.pdf img/sphere.pdf img/pki.pdf img/homo.pdf
	img/caesar.pdf img/ctr.pdf img/ind-atk.pdf img/ind-cca.pdf
	img/ec-op.pdf img/tile.pdf img/ec-neg-add.pdf img/pappus.pdf
	img/zero.pdf img/miller-line.pdf img/proxy0.pdf img/proxy.pdf
	img/inner-product.pdf img/mtorsion.pdf img/ec-zero.pdf
	img/dlin.pdf img/ec_walk.pdf img/base.pdf img/abe.pdf
	img/contents.pdf img/gcd.pdf img/rlwe.pdf)
set(IMAGES img/dh img/dh-ok img/dh-ng img/elgamal-weak
	img/circle img/dq3 img/dq3-2
	img/elliptic img/pairing img/timecapsule
	img/kusuri img/tversky img/ss img/extract
	img/access-tree img/no-pfs img/pfs
	img/birthday img/dh-oracle img/cbc img/cbc-dec
	img/combi img/area img/ec-add img/p1 img/rho
	img/mac img/iterated-hash img/bad-hmac img/protocol
	img/broad1 img/broad-tree img/broad-revocation
	img/easy-traitor img/traitor img/bdhe img/pairing2
	img/distortion img/pairing-def img/mov img/mov2
	img/peks1 img/peks2 img/vote img/blind img/psi
	img/stream img/tokei img/sphere img/pki img/homo
	img/caesar img/ctr img/ind-atk img/ind-cca
	img/ec-op img/tile img/ec-neg-add img/pappus
	img/zero img/miller-line img/proxy0 img/proxy
	img/inner-product img/mtorsion img/ec-zero
	img/dlin img/ec_walk img/base img/abe
	img/contents img/gcd img/rlwe)


set(ADD_IDX ./add_idx.py)

find_package(LATEX REQUIRED)
find_package(LatexMk REQUIRED)
find_package(PythonInterp REQUIRED)

foreach(f ${TEX_FILES})
  add_custom_command(
    OUTPUT _${f}
    COMMAND ${Python_EXECUTABLE}
    ARGS ${ADD_IDX} ${f}
    DEPENDS ${ADD_IDX} ${f}
  )
endforeach()

foreach(f ${IMAGES})
  add_custom_command(
    OUTPUT ${f}.pdf
    COMMAND ./pptx2pdf.vbs
    ARGS ${f}.pptx
    COMMAND pdfcrop
    DEPENDS ${f}.pptx
  )
endforeach()  

add_custom_command(
  OUTPUT ${BOOK_NAME}.pdf
  COMMAND ${LATEXMK_EXECUTABLE}
  ARGS ${LATEXMK_FLAGS} ${BOOK_NAME}.tex
  DEPENDS ${TEX_FILES} ${_TEX_FILES} ${PDF_IMAGES}
)

add_custom_target(main ALL echo
  DEPENDS ${BOOK_NAME}.pdf
)

add_custom_target(main-clean CLEAN echo
  COMMAND ${LATEXMK_EXECUTABLE}
  ARGS -C
)
