AC_INIT([libmooshika], [0.2], [dominique.martinet@cea.fr])
AM_INIT_AUTOMAKE([foreign])

AC_CONFIG_MACRO_DIR([m4])

AC_GNU_SOURCE
AC_PROG_CC
AC_PROG_LIBTOOL

# Check for InfiniBand Libs and Headers
AC_SEARCH_LIBS([ibv_create_qp], [ibverbs], [], [
	AC_MSG_ERROR([unable to find libibverbs])
])
AC_CHECK_HEADERS([infiniband/verbs.h],[], [AC_MSG_ERROR(missing infiniband headers)])

# Check for RDAM_CM Libs and Headers
AC_SEARCH_LIBS([rdma_create_id], [rdmacm], [], [
	AC_MSG_ERROR([unable to find librdmacm])
])
AC_CHECK_HEADERS([rdma/rdma_cma.h],[], [AC_MSG_ERROR(missing rdma headers)])

AC_DEFINE( [VERSION_COMMENT], ["libmooshika and examples"], [No Comment])

# Git latest commit
AC_MSG_CHECKING( [Git HEAD] )
head_commit=`git rev-parse HEAD 2>/dev/null`

if test "x$head_commit" == "x" ; then
  AC_MSG_RESULT( [no git here] ) 
  AC_DEFINE_UNQUOTED( [_GIT_HEAD_COMMIT], "not compiled within a git repository", [Lastest HEAD at the time configure was run])
  AC_MSG_CHECKING( [Git describe] )
  AC_DEFINE_UNQUOTED( [_GIT_DESCRIBE], "not compiled within a git repository", [Result of git-describe --long])
  AC_SUBST(_GIT_HEAD_COMMIT_ABBREV)
else
  AC_MSG_RESULT( $head_commit ) 
  AC_DEFINE_UNQUOTED( [_GIT_HEAD_COMMIT], "$head_commit", [Lastest HEAD at the time configure was run])
  AC_MSG_CHECKING( [Git describe] )
  git_describe=`git describe --long`
  AC_MSG_RESULT( $git_describe ) 
  AC_DEFINE_UNQUOTED( [_GIT_DESCRIBE], "$git_describe", [Result of git-describe --long])
  AC_SUBST(_GIT_HEAD_COMMIT_ABBREV, .${head_commit:0:7})
fi


AC_ARG_ENABLE(rmitm,
	AC_HELP_STRING([--enable-rmitm], [Enable rmitm compilation, requires pcap development files (auto)]))

if test x$enable_rmitm != xno ; then
	AC_CHECK_HEADERS([pcap/pcap.h], [ enable_rmitm=yes ], [if test x$enable_rmitm != x; then AC_MSG_ERROR(rmitm requires libpcap); else echo "libpcap development files not found, disabling rmitm"; fi])
fi
AM_CONDITIONAL(ENABLE_RMITM, test x$enable_rmitm = xyes)

#for exporting to spec file
AC_SUBST(ac_configure_args)

AC_CONFIG_HEADERS([include/config.h])
AC_CONFIG_FILES([Makefile src/Makefile src/tests/Makefile src/libmooshika.pc libmooshika.spec])
AC_OUTPUT
