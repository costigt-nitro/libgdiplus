cmake_minimum_required(VERSION 3.12.2)

project(libgdiplus)

set(CMAKE_CONFIGURATION_TYPES "Debug;Release"
    CACHE STRING "Configuration types" FORCE)

add_library(libgdiplus SHARED
  config.h
  adjustablearrowcap.c
  adjustablearrowcap.h
  adjustablearrowcap-private.h
  bitmap.c
  bitmap.h
  bitmap-private.h
  bmpcodec.c
  bmpcodec.h
  brush.c
  brush.h
  brush-private.h
  carbon-private.c
  carbon-private.h
  codecs.h
  codecs-private.h
  customlinecap.c
  customlinecap.h
  customlinecap-private.h
  dstream.c
  dstream.h
  emfcodec.c
  emfcodec.h
  emfplus.c
  emfplus.h
  font.c
  font.h
  fontcollection.h
  fontcollection-private.h
  fontfamily.h
  fontfamily-private.h
  font-private.h
  gdi32.c
  gdi32.h
  gdip.h
  gdipenums.h
  GdiPlusFlat.h
  gdiplusimaging.h
  gdipluspixelformats.h
  gdiplus-private.h
  gdipstructs.h
  general.c
  general.h
  general-private.h
  gifcodec.c
  gifcodec.h
  graphics.c
  graphics.h
  graphics-cairo.c
  graphics-cairo-private.h
  graphics-metafile.c
  graphics-metafile-private.h
  graphics-path.c
  graphics-path.h
  graphics-pathiterator.c
  graphics-pathiterator.h
  graphics-pathiterator-private.h
  graphics-path-private.h
  graphics-private.h
  hatchbrush.c
  hatchbrush.h
  hatchbrush-private.h
  icocodec.c
  icocodec.h
  image.c
  image.h
  imageattributes.c
  imageattributes.h
  imageattributes-private.h
  image-private.h
  jpegcodec.c
  jpegcodec.h
  lineargradientbrush.c
  lineargradientbrush.h
  lineargradientbrush-private.h
  matrix.c
  matrix.h
  matrix-private.h
  metafile.c
  metafile.h
  metafile-private.h
  pathgradientbrush.c
  pathgradientbrush.h
  pathgradientbrush-private.h
  pen.c
  pen.h
  pen-private.h
  pngcodec.c
  pngcodec.h
  print.c
  region.c
  region.h
  region-bitmap.c
  region-bitmap.h
  region-path-tree.c
  region-path-tree.h
  region-private.h
  solidbrush.c
  solidbrush.h
  solidbrush-private.h
  stringformat.c
  stringformat.h
  stringformat-private.h
  text.c
  text.h
  text-cairo.c
  text-cairo-private.h
  text-metafile.c
  text-metafile-private.h
  text-pango.c
  text-pango-private.h
  texturebrush.c
  texturebrush.h
  texturebrush-private.h
  tiffcodec.c
  tiffcodec.h
  win32-private.c
  win32-private.h
  win32structs.h
  wmfcodec.c
  wmfcodec.h
)
# Additional include directories
set_property(TARGET libgdiplus
  APPEND PROPERTY INCLUDE_DIRECTORIES
  $<$<CONFIG:Debug>:
    ${CMAKE_SOURCE_DIR}>
  $<$<CONFIG:Release>:
    ${CMAKE_SOURCE_DIR}>
)

# Preprocessor definitions
target_compile_definitions(libgdiplus PRIVATE
  $<$<CONFIG:Debug>:_MBCS;HAVE_LIBGIF;HAVE_LIBJPEG;HAVE_LIBTIFF;HAVE_LIBPNG;HAVE_FCFINI;USE_PANGO_RENDERING;_WINDLL;WIN32>
  $<$<CONFIG:Release>:_MBCS;HAVE_LIBGIF;HAVE_LIBJPEG;HAVE_LIBTIFF;HAVE_LIBPNG;HAVE_FCFINI;USE_PANGO_RENDERING;_WINDLL;WIN32>
)

# Minimal rebuild
if (MSVC)
  target_compile_options(libgdiplus PRIVATE
    "$<$<CONFIG:Debug>:/Gm->"
    "$<$<CONFIG:Release>:/Gm->"
  )
endif ()

# Precompiled header files
if (MSVC)
  target_compile_options(libgdiplus PRIVATE
    ""
  )
endif ()

# Link libraries
set_property(TARGET libgdiplus
  APPEND PROPERTY LINK_LIBRARIES
  "$<$<CONFIG:Debug>:shcore.lib>"
  "$<$<CONFIG:Release>:shcore.lib>"
)

execute_process(
	COMMAND powershell.exe -NonInteractive -Command "(Get-Content \"${PROJECT_SOURCE_DIR}\\..\\winconfig.h.in\") -replace '#LIBGDIPLUS_VERSION#', (Select-String -path \"${PROJECT_SOURCE_DIR}\\..\\configure.ac\" -pattern 'AC_INIT\\(libgdiplus, \\[(.*)\\]').Matches[0].Groups[1].Value | Set-Content \"${PROJECT_SOURCE_DIR}\\config.h\""
)