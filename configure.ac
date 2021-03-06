dnl Process this file with autoconf to produce a configure script.

AC_INIT(epsilon, 0.3.0.012, enlightenment-devel@lists.sourceforge.net)
AC_PREREQ(2.52)
AC_CONFIG_SRCDIR(configure.ac)

AM_INIT_AUTOMAKE(1.6 dist-bzip2)
AM_CONFIG_HEADER(src/config.h)

AC_PROG_CC
AM_PROG_CC_STDC
AC_C_BIGENDIAN

AC_LIBTOOL_WIN32_DLL
define([AC_LIBTOOL_LANG_CXX_CONFIG], [:])dnl
define([AC_LIBTOOL_LANG_F77_CONFIG], [:])dnl
AC_PROG_LIBTOOL

VMAJ=`echo $PACKAGE_VERSION | awk -F. '{printf("%s", $1);}'`
VMIN=`echo $PACKAGE_VERSION | awk -F. '{printf("%s", $2);}'`
VMIC=`echo $PACKAGE_VERSION | awk -F. '{printf("%s", $3);}'`
SNAP=`echo $PACKAGE_VERSION | awk -F. '{printf("%s", $4);}'`
version_info=`expr $VMAJ + $VMIN`":$VMIC:$VMIN"
AC_SUBST(version_info)

PKG_CHECK_MODULES(EINA, [eina-0])

PKG_CHECK_MODULES(EVAS, [evas >= 0.9.9])

PKG_CHECK_MODULES(ECORE, [
    ecore-evas >= 0.9.9
    ecore-file >= 0.9.9
    ecore-ipc >= 0.9.9
  ]
)

PKG_CHECK_MODULES(EDJE, [edje >= 0.5.0])

dnl Make sure the png header/lib is available
PKG_CHECK_EXISTS(libpng12,
  [ PKG_CHECK_MODULES(PNG, libpng12, [ have_png="yes" ], [ have_png="no"]) ],
  [
    PKG_CHECK_EXISTS(libpng10,
      [ PKG_CHECK_MODULES(PNG, libpng10, [ have_png="yes" ], [ have_png="no"]) ],
      [ PKG_CHECK_MODULES(PNG, libpng, [ have_png="yes" ], [ have_png="no"]) ]
    )
  ]
)
if test X"$have_png" = Xyes; then
  AC_DEFINE(HAVE_PNG_H, 1, [png.h is available])
else
  AC_MSG_WARN(Cannot find libpng on your system: png thumbnail suckage!)
fi

AC_PATH_PROG(PERL, perl, 0)
AC_SUBST(PERL)

xine=""
AC_MSG_CHECKING(whether the xine thumbnailer was explicitly disabled)
AC_ARG_ENABLE(thumbnailer-xine, AC_HELP_STRING([--enable-thumbnailer-xine],[build the thumbnailer for videos]),
        [ ac_use_plugin_xine="$enableval" ],
        [ ac_use_plugin_xine="yes" ]
        )
if test X"$ac_use_plugin_xine" = Xyes; then
        AC_MSG_RESULT(no)
        PKG_CHECK_MODULES(XINE, [libxine >= 1.0.0],
          [
            AC_DEFINE(HAVE_LIBXINE, 1, build xinefile thumbnailer)
	    xine="xine"
	  ],
          [
            AC_MSG_WARN([libxine too old; no thumbnails for videos...])
          ]
        )
else
        AC_MSG_RESULT(yes)
        xine=""
fi
AM_CONDITIONAL(HAVE_XINE, test $xine = "xine")

requirements="evas ecore-evas ecore-file ecore-ipc edje"
AC_SUBST(requirements)

#####################################################################
## Output

AC_OUTPUT([
Makefile
epsilon.pc
epsilon.spec
src/Makefile
src/include/Makefile
src/lib/Makefile
src/lib/exiftags/Makefile
src/bin/Makefile
src/plugins/Makefile
])


#####################################################################
## Info

echo
echo
echo
echo "------------------------------------------------------------------------"
echo "$PACKAGE $VERSION"
echo "------------------------------------------------------------------------"
echo
echo "Configuration Options Summary:"
echo
echo "  Thumbnail support....:"
echo
echo "    freedesktop png....: $have_png"
echo
echo "  Compilation..........: make"
echo
echo "  Installation.........: make install"
echo
echo "    prefix.............: $prefix"
echo
