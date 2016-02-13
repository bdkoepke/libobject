cmake_minimum_required(VERSION 2.8)
include(noweb.cmake)
include(UseLatex)
set(libobject_C
    class.c
    object.c)
set(libobject_H
    class.h
    object.h)
set(libobject_TEX
    class.tex
    object.tex)

tangle("${libobject_H}")
tangle("${libobject_C}")
weave("${libobject_TEX}")
add_custom_target(tangle DEPENDS ${libobject_H} ${libobject_C})
add_custom_target(weave DEPENDS ${libobject_TEX})

add_library(object SHARED ${libobject_C})
add_dependencies(object tangle)

add_latex_document(libobject.tex
                   INPUTS preamble.tex
                   FORCE_PDF
                   TARGET_NAME libobject)
add_dependencies(libobject_pdf weave)
