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

add_library(libobject SHARED ${libobject_C})
add_dependencies(libobject tangle)
target_link_libraries(libobject m)

add_latex_document(libobject.tex
                   FORCE_PDF
                   TARGET_NAME _libobject)
add_dependencies(_libobject_pdf weave)
