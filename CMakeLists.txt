# Generate documentation in HTML and PDF format using Sphinx.
cmake_minimum_required(VERSION 2.8.12)
project(MyDumper)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)

OPTION(WITH_MAN "Build with man" ON)
OPTION(WITH_HTML "Build with html" ON)


# We use the Sphinx documentation generator to render HTML and manual
# pages from the user and reference documentation in ReST format.
find_package(Sphinx QUIET)
if(NOT SPHINX_FOUND)
  message(FATAL_ERROR "Unable to find Sphinx documentation generator")
endif(NOT SPHINX_FOUND)

if(SPHINX_MAJOR_VERSION LESS 1)
  message(FATAL_ERROR "Sphinx is older than v1.0, not building docs")
endif(SPHINX_MAJOR_VERSION LESS 1)

# documentation tools
set(SOURCE_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/_build")
# configured documentation tools and intermediate build results
set(BINARY_BUILD_DIR "${CMAKE_CURRENT_BINARY_DIR}/_build")
# static ReST documentation sources
set(SOURCES_DIR "${CMAKE_CURRENT_BINARY_DIR}/_sources")
# generated ReST documentation sources
set(REF_SOURCES_DIR "${SOURCES_DIR}/reference")
# master document with modules index
set(REF_MASTER_DOC "modules")

# substitute variables in configuration and scripts
foreach(file
    conf.py
    sources.cmake
)
  configure_file(
    "${SOURCE_BUILD_DIR}/${file}.in"
    "${BINARY_BUILD_DIR}/${file}"
    @ONLY
  )
endforeach(file)

message( " ${SPHINX_EXECUTABLE} ")

add_custom_target(ALL
  DEPENDS "${REF_SOURCES_DIR}/${REF_MASTER_DOC}.rst"
)

# Sphinx requires all sources in the same directory tree. As we wish
# to include generated reference documention from the build tree, we
# copy static ReST documents to the build tree before calling Sphinx.
add_custom_target(doc_sources ALL
  "${CMAKE_COMMAND}" -P "${BINARY_BUILD_DIR}/sources.cmake"
)
set(CLEAN_FILES
  "${SOURCES_DIR}"
)

# note the trailing slash to exclude directory name
install(DIRECTORY "${SOURCES_DIR}/"
  DESTINATION "share/doc/mydumper"
)

# Sphinx cache with pickled ReST documents
set(SPHINX_CACHE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_doctrees")

list(APPEND CLEAN_FILES
  "${SPHINX_CACHE_DIR}"
)

if (WITH_HTML)
  # Sphinx cache with pickled ReST documents
  set(SPHINX_CACHE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_doctrees")
  # HTML output directory
  set(SPHINX_HTML_DIR "${CMAKE_CURRENT_BINARY_DIR}/html")

  # This target builds HTML documentation using Sphinx.
  add_custom_target(doc_html ALL
    ${SPHINX_EXECUTABLE}
      -q -b html
      -c "${BINARY_BUILD_DIR}"
      -d "${SPHINX_CACHE_DIR}"
      "${SOURCES_DIR}"
      "${SPHINX_HTML_DIR}"
    COMMENT "Building HTML documentation with Sphinx"
  )

  add_dependencies(doc_html
    doc_sources
  )

  install(DIRECTORY "${SPHINX_HTML_DIR}"
    DESTINATION "share/doc/mydumper"
  )
endif()

list(APPEND CLEAN_FILES
  "${BINARY_BUILD_DIR}/html"
  "${SPHINX_HTML_DIR}"
)

# MAN Building
if (WITH_MAN)
  # MAN output directory
  set(SPHINX_MAN_DIR "${CMAKE_CURRENT_BINARY_DIR}/man")
  # This target builds a manual page using Sphinx.

  add_custom_target(doc_man ALL
    ${SPHINX_EXECUTABLE}
      -q -b man
      -c "${BINARY_BUILD_DIR}"
      -d "${SPHINX_CACHE_DIR}"
      "${SOURCES_DIR}"
      "${SPHINX_MAN_DIR}"
    COMMENT "Building manual page with Sphinx"
  )

  add_dependencies(doc_man
    doc_sources
  )

  # serialize Sphinx targets to avoid cache conflicts in parallel builds
  add_dependencies(doc_man
    doc_sources
  )

  install(FILES "${SPHINX_MAN_DIR}/mydumper.1" "${SPHINX_MAN_DIR}/myloader.1"
    DESTINATION "share/man/man1"
  )
endif()

list(APPEND CLEAN_FILES
  "${SPHINX_MAN_DIR}"
)

# This target builds PDF documentation using Sphinx and LaTeX.
if(PDFLATEX_COMPILER)
  # PDF output directory
  set(SPHINX_PDF_DIR "${CMAKE_CURRENT_BINARY_DIR}/pdf")

  add_custom_target(doc_pdf ALL
    ${SPHINX_EXECUTABLE}
      -q -b latex
      -c "${BINARY_BUILD_DIR}"
      -d "${SPHINX_CACHE_DIR}"
      "${SOURCES_DIR}"
      "${SPHINX_PDF_DIR}"
      COMMENT "Building PDF documentation with Sphinx"
    )
  add_custom_command(TARGET doc_pdf POST_BUILD
    COMMAND ${CMAKE_MAKE_PROGRAM} LATEXOPTS=-interaction=batchmode
    WORKING_DIRECTORY "${SPHINX_PDF_DIR}"
  )
  list(APPEND CLEAN_FILES
   "${SPHINX_PDF_DIR}"
    )
  add_dependencies(doc_pdf
    doc_sources
  )
  # serialize Sphinx targets to avoid cache conflicts in parallel builds
  add_dependencies(doc_pdf
    doc_man
  )
  install(FILES "${SPHINX_PDF_DIR}/mydumper.pdf"
    DESTINATION "share/doc/mydumper"
  )
endif(PDFLATEX_COMPILER)

# Add output directories to clean target.
set_directory_properties(PROPERTIES
  ADDITIONAL_MAKE_CLEAN_FILES "${CLEAN_FILES}"
)


MESSAGE(STATUS "------------------------------------------------")
MESSAGE(STATUS "WITH_HTML = ${WITH_HTML}")
MESSAGE(STATUS "WITH_MAN = ${WITH_MAN}")
MESSAGE(STATUS "PDFLATEX_COMPILER = ${PDFLATEX_COMPILER}")
MESSAGE(STATUS "Change a values with: cmake -D<Variable>=<Value>")
MESSAGE(STATUS "------------------------------------------------")
MESSAGE(STATUS)
