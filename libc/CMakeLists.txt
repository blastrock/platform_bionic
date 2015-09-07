enable_language(ASM)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/arch-x86_64/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../libm/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../libm/include/amd64)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/kernel/uapi)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/kernel/uapi/asm-x86)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/stdio)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99 -Wno-unknown-attributes -D__LP64__")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -nostdinc++ -Wno-unknown-attributes -fno-exceptions")

set(libc_common_cflags "-D_LIBC=1 -Wall -Wextra -Wunused")

set(libc_common_cflags "${libc_common_cflags} -DUSE_DLMALLOC")
set(libc_malloc_src bionic/dlmalloc.c)

set(libc_common_cflags "${libc_common_cflags} -DANDROID_SMP=0")

set(libc_common_src_files
    bionic/bindresvport.c
    bionic/daemon.c
    bionic/err.c
    bionic/ether_aton.c
    bionic/ether_ntoa.c
    bionic/fts.c
    bionic/gethostname.c
    bionic/getpriority.c
    bionic/if_indextoname.c
    bionic/if_nametoindex.c
    bionic/initgroups.c
    bionic/ioctl.c
    bionic/isatty.c
    bionic/memmem.c
    bionic/pathconf.c
    bionic/pututline.c
    bionic/sched_cpualloc.c
    bionic/sched_cpucount.c
    bionic/semaphore.c
    bionic/sigblock.c
    bionic/siginterrupt.c
    bionic/sigsetmask.c
    bionic/system_properties_compat.c
    stdio/findfp.c
    stdio/fread.c
    stdio/snprintf.c
    stdio/sprintf.c
    )

# Fortify implementations of libc functions.
set(libc_common_src_files ${libc_common_src_files}
    bionic/__FD_chk.cpp
    bionic/__fgets_chk.cpp
    bionic/__memmove_chk.cpp
    bionic/__read_chk.cpp
    bionic/__recvfrom_chk.cpp
    bionic/__stpcpy_chk.cpp
    bionic/__stpncpy_chk.cpp
    bionic/__strchr_chk.cpp
    bionic/__strlcat_chk.cpp
    bionic/__strlcpy_chk.cpp
    bionic/__strlen_chk.cpp
    bionic/__strncat_chk.cpp
    bionic/__strncpy_chk.cpp
    bionic/__strrchr_chk.cpp
    bionic/__umask_chk.cpp
    bionic/__vsnprintf_chk.cpp
    bionic/__vsprintf_chk.cpp
    )

set(libc_bionic_src_files
    bionic/abort.cpp
    bionic/accept.cpp
    bionic/accept4.cpp
    bionic/access.cpp
    bionic/assert.cpp
    bionic/atof.cpp
    bionic/bionic_time_conversions.cpp
    bionic/brk.cpp
    bionic/c16rtomb.cpp
    bionic/c32rtomb.cpp
    bionic/chmod.cpp
    bionic/chown.cpp
    bionic/clearenv.cpp
    bionic/clock.cpp
    bionic/clone.cpp
    bionic/__cmsg_nxthdr.cpp
    bionic/connect.cpp
    bionic/ctype.cpp
    bionic/dirent.cpp
    bionic/dup2.cpp
    bionic/epoll_create.cpp
    bionic/epoll_pwait.cpp
    bionic/epoll_wait.cpp
    bionic/__errno.cpp
    bionic/eventfd_read.cpp
    bionic/eventfd_write.cpp
    bionic/ffs.cpp
    bionic/flockfile.cpp
    bionic/fork.cpp
    bionic/fpclassify.cpp
    bionic/futimens.cpp
    bionic/getauxval.cpp
    bionic/getcwd.cpp
    bionic/getentropy_linux.c
    bionic/getpgrp.cpp
    bionic/getpid.cpp
    bionic/gettid.cpp
    bionic/inotify_init.cpp
    bionic/lchown.cpp
    bionic/lfs64_support.cpp
    bionic/__libc_current_sigrtmax.cpp
    bionic/__libc_current_sigrtmin.cpp
    bionic/libc_init_common.cpp
    bionic/libc_logging.cpp
    bionic/libgen.cpp
    bionic/link.cpp
    bionic/locale.cpp
    bionic/lstat.cpp
    bionic/mbrtoc16.cpp
    bionic/mbrtoc32.cpp
    bionic/mbstate.cpp
    bionic/mkdir.cpp
    bionic/mkfifo.cpp
    bionic/mknod.cpp
    bionic/mntent.cpp
    bionic/NetdClientDispatch.cpp
    bionic/open.cpp
    bionic/pause.cpp
    bionic/pipe.cpp
    bionic/poll.cpp
    bionic/posix_fadvise.cpp
    bionic/posix_fallocate.cpp
    bionic/posix_timers.cpp
    bionic/pthread_atfork.cpp
    bionic/pthread_attr.cpp
    bionic/pthread_cond.cpp
    bionic/pthread_create.cpp
    bionic/pthread_detach.cpp
    bionic/pthread_equal.cpp
    bionic/pthread_exit.cpp
    bionic/pthread_getcpuclockid.cpp
    bionic/pthread_getschedparam.cpp
    bionic/pthread_gettid_np.cpp
    bionic/pthread_internals.cpp
    bionic/pthread_join.cpp
    bionic/pthread_key.cpp
    bionic/pthread_kill.cpp
    bionic/pthread_mutex.cpp
    bionic/pthread_once.cpp
    bionic/pthread_rwlock.cpp
    bionic/pthread_self.cpp
    bionic/pthread_setname_np.cpp
    bionic/pthread_setschedparam.cpp
    bionic/pthread_sigmask.cpp
    bionic/ptrace.cpp
    bionic/pty.cpp
    bionic/raise.cpp
    bionic/rand.cpp
    bionic/readlink.cpp
    bionic/reboot.cpp
    bionic/recv.cpp
    bionic/rename.cpp
    bionic/rmdir.cpp
    bionic/scandir.cpp
    bionic/sched_getaffinity.cpp
    bionic/sched_getcpu.cpp
    bionic/send.cpp
    bionic/setegid.cpp
    bionic/__set_errno.cpp
    bionic/seteuid.cpp
    bionic/setpgrp.cpp
    bionic/sigaction.cpp
    bionic/sigaddset.cpp
    bionic/sigdelset.cpp
    bionic/sigemptyset.cpp
    bionic/sigfillset.cpp
    bionic/sigismember.cpp
    bionic/signal.cpp
    bionic/signalfd.cpp
    bionic/sigpending.cpp
    bionic/sigprocmask.cpp
    bionic/sigsuspend.cpp
    bionic/sigwait.cpp
    bionic/socket.cpp
    bionic/stat.cpp
    bionic/statvfs.cpp
    bionic/strcoll_l.cpp
    bionic/strerror.cpp
    bionic/strerror_r.cpp
    bionic/strftime_l.cpp
    bionic/strsignal.cpp
    bionic/strtold.cpp
    bionic/strtold_l.cpp
    bionic/strtoll_l.cpp
    bionic/strtoull_l.cpp
    bionic/strxfrm_l.cpp
    #bionic/stubs.cpp
    bionic/mystubs.cpp
    bionic/symlink.cpp
    bionic/sysconf.cpp
    bionic/syslog.cpp
    bionic/sys_siglist.c
    bionic/sys_signame.c
    #bionic/system_properties.cpp
    bionic/tdestroy.cpp
    bionic/termios.cpp
    bionic/thread_private.cpp
    bionic/tmpfile.cpp
    bionic/umount.cpp
    bionic/unlink.cpp
    bionic/utimes.cpp
    bionic/vdso.cpp
    bionic/wait.cpp
    bionic/wchar.cpp
    bionic/wctype.cpp
)

set(libc_cxa_src_files
    bionic/__cxa_guard.cpp
    bionic/__cxa_pure_virtual.cpp
    bionic/new.cpp
    )

set(libc_upstream_freebsd_src_files
    upstream-freebsd/lib/libc/gen/ldexp.c
    upstream-freebsd/lib/libc/gen/sleep.c
    upstream-freebsd/lib/libc/gen/usleep.c
    upstream-freebsd/lib/libc/stdio/fclose.c
    upstream-freebsd/lib/libc/stdio/flags.c
    upstream-freebsd/lib/libc/stdio/fopen.c
    upstream-freebsd/lib/libc/stdlib/abs.c
    upstream-freebsd/lib/libc/stdlib/getopt_long.c
    upstream-freebsd/lib/libc/stdlib/imaxabs.c
    upstream-freebsd/lib/libc/stdlib/imaxdiv.c
    upstream-freebsd/lib/libc/stdlib/labs.c
    upstream-freebsd/lib/libc/stdlib/llabs.c
    upstream-freebsd/lib/libc/stdlib/qsort.c
    upstream-freebsd/lib/libc/stdlib/quick_exit.c
    upstream-freebsd/lib/libc/stdlib/realpath.c
    upstream-freebsd/lib/libc/string/wcpcpy.c
    upstream-freebsd/lib/libc/string/wcpncpy.c
    upstream-freebsd/lib/libc/string/wcscasecmp.c
    upstream-freebsd/lib/libc/string/wcscspn.c
    upstream-freebsd/lib/libc/string/wcsdup.c
    upstream-freebsd/lib/libc/string/wcslcat.c
    upstream-freebsd/lib/libc/string/wcsncasecmp.c
    upstream-freebsd/lib/libc/string/wcsncat.c
    upstream-freebsd/lib/libc/string/wcsncmp.c
    upstream-freebsd/lib/libc/string/wcsncpy.c
    upstream-freebsd/lib/libc/string/wcsnlen.c
    upstream-freebsd/lib/libc/string/wcspbrk.c
    upstream-freebsd/lib/libc/string/wcsspn.c
    upstream-freebsd/lib/libc/string/wcstok.c
    upstream-freebsd/lib/libc/string/wmemchr.c
    upstream-freebsd/lib/libc/string/wmemcpy.c
    upstream-freebsd/lib/libc/string/wmemset.c
    )

set(libc_upstream_netbsd_src_files
    upstream-netbsd/common/lib/libc/stdlib/random.c
    upstream-netbsd/lib/libc/gen/ftw.c
    upstream-netbsd/lib/libc/gen/nftw.c
    upstream-netbsd/lib/libc/gen/nice.c
    upstream-netbsd/lib/libc/gen/popen.c
    upstream-netbsd/lib/libc/gen/psignal.c
    upstream-netbsd/lib/libc/gen/utime.c
    upstream-netbsd/lib/libc/gen/utmp.c
    upstream-netbsd/lib/libc/inet/nsap_addr.c
    upstream-netbsd/lib/libc/regex/regcomp.c
    upstream-netbsd/lib/libc/regex/regerror.c
    upstream-netbsd/lib/libc/regex/regexec.c
    upstream-netbsd/lib/libc/regex/regfree.c
    upstream-netbsd/lib/libc/stdlib/bsearch.c
    upstream-netbsd/lib/libc/stdlib/div.c
    upstream-netbsd/lib/libc/stdlib/drand48.c
    upstream-netbsd/lib/libc/stdlib/erand48.c
    upstream-netbsd/lib/libc/stdlib/insque.c
    upstream-netbsd/lib/libc/stdlib/jrand48.c
    upstream-netbsd/lib/libc/stdlib/ldiv.c
    upstream-netbsd/lib/libc/stdlib/lldiv.c
    upstream-netbsd/lib/libc/stdlib/lrand48.c
    upstream-netbsd/lib/libc/stdlib/mrand48.c
    upstream-netbsd/lib/libc/stdlib/nrand48.c
    upstream-netbsd/lib/libc/stdlib/_rand48.c
    upstream-netbsd/lib/libc/stdlib/rand_r.c
    upstream-netbsd/lib/libc/stdlib/remque.c
    upstream-netbsd/lib/libc/stdlib/seed48.c
    upstream-netbsd/lib/libc/stdlib/srand48.c
    upstream-netbsd/lib/libc/string/memccpy.c
    upstream-netbsd/lib/libc/string/strcasestr.c
    upstream-netbsd/lib/libc/string/strcoll.c
    upstream-netbsd/lib/libc/string/strxfrm.c
    upstream-netbsd/lib/libc/unistd/killpg.c
    )

set(libc_upstream_openbsd_gdtoa_src_files
    upstream-openbsd/android/gdtoa_support.cpp
    upstream-openbsd/lib/libc/gdtoa/dmisc.c
    upstream-openbsd/lib/libc/gdtoa/dtoa.c
    upstream-openbsd/lib/libc/gdtoa/gdtoa.c
    upstream-openbsd/lib/libc/gdtoa/gethex.c
    upstream-openbsd/lib/libc/gdtoa/gmisc.c
    upstream-openbsd/lib/libc/gdtoa/hd_init.c
    upstream-openbsd/lib/libc/gdtoa/hdtoa.c
    upstream-openbsd/lib/libc/gdtoa/hexnan.c
    upstream-openbsd/lib/libc/gdtoa/ldtoa.c
    upstream-openbsd/lib/libc/gdtoa/misc.c
    upstream-openbsd/lib/libc/gdtoa/smisc.c
    upstream-openbsd/lib/libc/gdtoa/strtod.c
    upstream-openbsd/lib/libc/gdtoa/strtodg.c
    upstream-openbsd/lib/libc/gdtoa/strtof.c
    upstream-openbsd/lib/libc/gdtoa/strtord.c
    upstream-openbsd/lib/libc/gdtoa/sum.c
    upstream-openbsd/lib/libc/gdtoa/ulp.c
    )

set(libc_upstream_openbsd_gdtoa_src_files_32
    ${libc_upstream_openbsd_gdtoa_src_files}
    )

set(libc_upstream_openbsd_gdtoa_src_files_64
    ${libc_upstream_openbsd_gdtoa_src_files}
    upstream-openbsd/lib/libc/gdtoa/strtorQ.c
    )

set(libc_upstream_openbsd_src_files
    upstream-openbsd/lib/libc/crypt/arc4random.c
    upstream-openbsd/lib/libc/crypt/arc4random_uniform.c
    upstream-openbsd/lib/libc/gen/alarm.c
    upstream-openbsd/lib/libc/gen/ctype_.c
    upstream-openbsd/lib/libc/gen/exec.c
    upstream-openbsd/lib/libc/gen/fnmatch.c
    upstream-openbsd/lib/libc/gen/ftok.c
    upstream-openbsd/lib/libc/gen/getprogname.c
    upstream-openbsd/lib/libc/gen/isctype.c
    upstream-openbsd/lib/libc/gen/setprogname.c
    upstream-openbsd/lib/libc/gen/time.c
    upstream-openbsd/lib/libc/gen/tolower_.c
    upstream-openbsd/lib/libc/gen/toupper_.c
    upstream-openbsd/lib/libc/locale/btowc.c
    upstream-openbsd/lib/libc/locale/mbrlen.c
    upstream-openbsd/lib/libc/locale/mbstowcs.c
    upstream-openbsd/lib/libc/locale/mbtowc.c
    upstream-openbsd/lib/libc/locale/wcscoll.c
    upstream-openbsd/lib/libc/locale/wcstod.c
    upstream-openbsd/lib/libc/locale/wcstof.c
    upstream-openbsd/lib/libc/locale/wcstoimax.c
    upstream-openbsd/lib/libc/locale/wcstol.c
    upstream-openbsd/lib/libc/locale/wcstold.c
    upstream-openbsd/lib/libc/locale/wcstoll.c
    upstream-openbsd/lib/libc/locale/wcstombs.c
    upstream-openbsd/lib/libc/locale/wcstoul.c
    upstream-openbsd/lib/libc/locale/wcstoull.c
    upstream-openbsd/lib/libc/locale/wcstoumax.c
    upstream-openbsd/lib/libc/locale/wcsxfrm.c
    upstream-openbsd/lib/libc/locale/wctob.c
    upstream-openbsd/lib/libc/locale/wctomb.c
    upstream-openbsd/lib/libc/net/htonl.c
    upstream-openbsd/lib/libc/net/htons.c
    upstream-openbsd/lib/libc/net/inet_addr.c
    upstream-openbsd/lib/libc/net/inet_lnaof.c
    upstream-openbsd/lib/libc/net/inet_makeaddr.c
    upstream-openbsd/lib/libc/net/inet_netof.c
    upstream-openbsd/lib/libc/net/inet_network.c
    upstream-openbsd/lib/libc/net/inet_ntoa.c
    upstream-openbsd/lib/libc/net/inet_ntop.c
    upstream-openbsd/lib/libc/net/inet_pton.c
    upstream-openbsd/lib/libc/net/ntohl.c
    upstream-openbsd/lib/libc/net/ntohs.c
    upstream-openbsd/lib/libc/stdio/asprintf.c
    upstream-openbsd/lib/libc/stdio/clrerr.c
    upstream-openbsd/lib/libc/stdio/dprintf.c
    upstream-openbsd/lib/libc/stdio/fdopen.c
    upstream-openbsd/lib/libc/stdio/feof.c
    upstream-openbsd/lib/libc/stdio/ferror.c
    upstream-openbsd/lib/libc/stdio/fflush.c
    upstream-openbsd/lib/libc/stdio/fgetc.c
    upstream-openbsd/lib/libc/stdio/fgetln.c
    upstream-openbsd/lib/libc/stdio/fgetpos.c
    upstream-openbsd/lib/libc/stdio/fgets.c
    upstream-openbsd/lib/libc/stdio/fgetwc.c
    upstream-openbsd/lib/libc/stdio/fgetws.c
    upstream-openbsd/lib/libc/stdio/fileno.c
    upstream-openbsd/lib/libc/stdio/fprintf.c
    upstream-openbsd/lib/libc/stdio/fpurge.c
    upstream-openbsd/lib/libc/stdio/fputc.c
    upstream-openbsd/lib/libc/stdio/fputs.c
    upstream-openbsd/lib/libc/stdio/fputwc.c
    upstream-openbsd/lib/libc/stdio/fputws.c
    upstream-openbsd/lib/libc/stdio/freopen.c
    upstream-openbsd/lib/libc/stdio/fscanf.c
    upstream-openbsd/lib/libc/stdio/fseek.c
    upstream-openbsd/lib/libc/stdio/fsetpos.c
    upstream-openbsd/lib/libc/stdio/ftell.c
    upstream-openbsd/lib/libc/stdio/funopen.c
    upstream-openbsd/lib/libc/stdio/fvwrite.c
    upstream-openbsd/lib/libc/stdio/fwalk.c
    upstream-openbsd/lib/libc/stdio/fwide.c
    upstream-openbsd/lib/libc/stdio/fwprintf.c
    upstream-openbsd/lib/libc/stdio/fwrite.c
    upstream-openbsd/lib/libc/stdio/fwscanf.c
    upstream-openbsd/lib/libc/stdio/getc.c
    upstream-openbsd/lib/libc/stdio/getchar.c
    upstream-openbsd/lib/libc/stdio/getdelim.c
    upstream-openbsd/lib/libc/stdio/getline.c
    upstream-openbsd/lib/libc/stdio/gets.c
    upstream-openbsd/lib/libc/stdio/getwc.c
    upstream-openbsd/lib/libc/stdio/getwchar.c
    upstream-openbsd/lib/libc/stdio/makebuf.c
    upstream-openbsd/lib/libc/stdio/mktemp.c
    upstream-openbsd/lib/libc/stdio/perror.c
    upstream-openbsd/lib/libc/stdio/printf.c
    upstream-openbsd/lib/libc/stdio/putc.c
    upstream-openbsd/lib/libc/stdio/putchar.c
    upstream-openbsd/lib/libc/stdio/puts.c
    upstream-openbsd/lib/libc/stdio/putwc.c
    upstream-openbsd/lib/libc/stdio/putwchar.c
    upstream-openbsd/lib/libc/stdio/refill.c
    upstream-openbsd/lib/libc/stdio/remove.c
    upstream-openbsd/lib/libc/stdio/rewind.c
    upstream-openbsd/lib/libc/stdio/rget.c
    upstream-openbsd/lib/libc/stdio/scanf.c
    upstream-openbsd/lib/libc/stdio/setbuf.c
    upstream-openbsd/lib/libc/stdio/setbuffer.c
    upstream-openbsd/lib/libc/stdio/setvbuf.c
    upstream-openbsd/lib/libc/stdio/sscanf.c
    upstream-openbsd/lib/libc/stdio/stdio.c
    upstream-openbsd/lib/libc/stdio/swprintf.c
    upstream-openbsd/lib/libc/stdio/swscanf.c
    upstream-openbsd/lib/libc/stdio/tempnam.c
    upstream-openbsd/lib/libc/stdio/tmpnam.c
    upstream-openbsd/lib/libc/stdio/ungetc.c
    upstream-openbsd/lib/libc/stdio/ungetwc.c
    upstream-openbsd/lib/libc/stdio/vasprintf.c
    upstream-openbsd/lib/libc/stdio/vdprintf.c
    upstream-openbsd/lib/libc/stdio/vfprintf.c
    upstream-openbsd/lib/libc/stdio/vfscanf.c
    upstream-openbsd/lib/libc/stdio/vfwprintf.c
    upstream-openbsd/lib/libc/stdio/vfwscanf.c
    upstream-openbsd/lib/libc/stdio/vprintf.c
    upstream-openbsd/lib/libc/stdio/vscanf.c
    upstream-openbsd/lib/libc/stdio/vsnprintf.c
    upstream-openbsd/lib/libc/stdio/vsprintf.c
    upstream-openbsd/lib/libc/stdio/vsscanf.c
    upstream-openbsd/lib/libc/stdio/vswprintf.c
    upstream-openbsd/lib/libc/stdio/vswscanf.c
    upstream-openbsd/lib/libc/stdio/vwprintf.c
    upstream-openbsd/lib/libc/stdio/vwscanf.c
    upstream-openbsd/lib/libc/stdio/wbuf.c
    upstream-openbsd/lib/libc/stdio/wprintf.c
    upstream-openbsd/lib/libc/stdio/wscanf.c
    upstream-openbsd/lib/libc/stdio/wsetup.c
    upstream-openbsd/lib/libc/stdlib/atexit.c
    upstream-openbsd/lib/libc/stdlib/atoi.c
    upstream-openbsd/lib/libc/stdlib/atol.c
    upstream-openbsd/lib/libc/stdlib/atoll.c
    upstream-openbsd/lib/libc/stdlib/exit.c
    upstream-openbsd/lib/libc/stdlib/getenv.c
    upstream-openbsd/lib/libc/stdlib/lsearch.c
    upstream-openbsd/lib/libc/stdlib/setenv.c
    upstream-openbsd/lib/libc/stdlib/strtoimax.c
    upstream-openbsd/lib/libc/stdlib/strtol.c
    upstream-openbsd/lib/libc/stdlib/strtoll.c
    upstream-openbsd/lib/libc/stdlib/strtoul.c
    upstream-openbsd/lib/libc/stdlib/strtoull.c
    upstream-openbsd/lib/libc/stdlib/strtoumax.c
    upstream-openbsd/lib/libc/stdlib/system.c
    upstream-openbsd/lib/libc/stdlib/tfind.c
    upstream-openbsd/lib/libc/stdlib/tsearch.c
    upstream-openbsd/lib/libc/string/strcasecmp.c
    upstream-openbsd/lib/libc/string/strcspn.c
    upstream-openbsd/lib/libc/string/strdup.c
    upstream-openbsd/lib/libc/string/strndup.c
    upstream-openbsd/lib/libc/string/strpbrk.c
    upstream-openbsd/lib/libc/string/strsep.c
    upstream-openbsd/lib/libc/string/strspn.c
    upstream-openbsd/lib/libc/string/strstr.c
    upstream-openbsd/lib/libc/string/strtok.c
    upstream-openbsd/lib/libc/string/wcslcpy.c
    upstream-openbsd/lib/libc/string/wcsstr.c
    upstream-openbsd/lib/libc/string/wcswidth.c
    )

set(libc_arch_static_src_files
    bionic/dl_iterate_phdr_static.cpp
    )

# 64-bit x86.

#
# Default implementations of functions that are commonly optimized.
#

set(libc_bionic_src_files_x86_64
    bionic/__memcpy_chk.cpp
    bionic/__memset_chk.cpp
    bionic/__strcpy_chk.cpp
    bionic/__strcat_chk.cpp
    bionic/memchr.c
    bionic/memrchr.c
    bionic/strchr.cpp
    bionic/strnlen.c
    bionic/strrchr.cpp
    )

set(libc_freebsd_src_files_x86_64
    upstream-freebsd/lib/libc/string/wcscat.c
    upstream-freebsd/lib/libc/string/wcschr.c
    upstream-freebsd/lib/libc/string/wcscmp.c
    upstream-freebsd/lib/libc/string/wcscpy.c
    upstream-freebsd/lib/libc/string/wcslen.c
    upstream-freebsd/lib/libc/string/wcsrchr.c
    upstream-freebsd/lib/libc/string/wmemcmp.c
    upstream-freebsd/lib/libc/string/wmemmove.c
    )

set(libc_openbsd_src_files_x86_64
    upstream-openbsd/lib/libc/string/strlcat.c
    upstream-openbsd/lib/libc/string/strlcpy.c
    )

#
# Inherently architecture-specific code.
#

set(libc_bionic_src_files_x86_64
    ${libc_bionic_src_files_x86_64}
    arch-x86_64/bionic/__bionic_clone.S
    arch-x86_64/bionic/_exit_with_stack_teardown.S
    arch-x86_64/bionic/__restore_rt.S
    arch-x86_64/bionic/_setjmp.S
    arch-x86_64/bionic/setjmp.S
    arch-x86_64/bionic/__set_tls.c
    arch-x86_64/bionic/sigsetjmp.S
    arch-x86_64/bionic/syscall.S
    arch-x86_64/bionic/vfork.S
    )

#
# Optimized memory/string functions.
#

set(libc_bionic_src_files_x86_64
    ${libc_bionic_src_files_x86_64}
    arch-x86_64/string/sse2-memcpy-slm.S
    arch-x86_64/string/sse2-memmove-slm.S
    arch-x86_64/string/sse2-memset-slm.S
    arch-x86_64/string/sse2-stpcpy-slm.S
    arch-x86_64/string/sse2-stpncpy-slm.S
    arch-x86_64/string/sse2-strcat-slm.S
    arch-x86_64/string/sse2-strcpy-slm.S
    arch-x86_64/string/sse2-strlen-slm.S
    arch-x86_64/string/sse2-strncat-slm.S
    arch-x86_64/string/sse2-strncpy-slm.S
    arch-x86_64/string/sse4-memcmp-slm.S
    arch-x86_64/string/ssse3-strcmp-slm.S
    arch-x86_64/string/ssse3-strncmp-slm.S
    )

set(libc_crt_target_cflags_x86_64
    -m64
    -I${CMAKE_CURRENT_SOURCE_DIR}/arch-x86_64/include
    )

set(libc_crt_target_ldflags_x86_64 -melf_x86_64)

set(libc_crt_target_crtbegin_file_x86_64 ${CMAKE_CURRENT_SOURCE_DIR}/arch-common/bionic/crtbegin.c)

set(libc_crt_target_crtbegin_so_file_x86_64 ${CMAKE_CURRENT_SOURCE_DIR}/arch-common/bionic/crtbegin_so.c)

set(libc_crt_target_so_cflags_x86_64 -fPIC)

file(GLOB syscalls_src "arch-x86_64/syscalls/*.S")

file(GLOB libc_tzcode_src_files "tzcode/*.c")

add_library(c_syscalls
  STATIC
  ${syscalls_src}
  )

add_library(c_cxa
  STATIC
  ${libc_cxa_src_files}
  )
set_target_properties(c_cxa PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -fvisibility=hidden")
target_include_directories(c_cxa PRIVATE ../libstdc++/include)

add_library(c_tzcode
  STATIC
  ${libc_tzcode_src_files}
  )
set_target_properties(c_tzcode PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -fvisibility=hidden -DALL_STATE -DSTD_INSPIRED -DTM_GMTOFF=tm_gmtoff -DTZDIR=\"/usr/share/zoneinfo\" -DUSG_COMPAT=1 -DNO_RUN_TIME_WARNINGS_ABOUT_YEAR_2000_PROBLEMS_THANK_YOU -Dlint")

add_library(c_freebsd
  STATIC
  ${libc_upstream_freebsd_src_files}
  ${libc_freebsd_src_files_x86_64}
  )
set_target_properties(c_freebsd PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -Wno-sign-compare -Wno-uninitialized -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-freebsd/android/include -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-freebsd/lib/libc/include -include freebsd-compat.h")

add_library(c_netbsd
  STATIC
  ${libc_upstream_netbsd_src_files}
  )
set_target_properties(c_netbsd PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -Wno-sign-compare -Wno-uninitialized -DPOSIX_MISTAKE -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-netbsd/android/include -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-netbsd/lib/libc/include -include netbsd-compat.h")

add_library(c_openbsd
  STATIC
  ${libc_upstream_openbsd_src_files}
  ${libc_openbsd_src_files_x86_64}
  )
set_target_properties(c_openbsd PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -Wno-sign-compare -Wno-uninitialized -Wno-unused-parameter -I${CMAKE_CURRENT_SOURCE_DIR}/private -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-openbsd/android/include -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-openbsd/lib/libc/include -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-openbsd/lib/libc/gdtoa/ -include openbsd-compat.h")

add_library(c_gdtoa
  STATIC
  ${libc_upstream_openbsd_gdtoa_src_files_64}
  )
set_target_properties(c_gdtoa PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -Wno-sign-compare -Wno-uninitialized -fvisibility=hidden -I${CMAKE_CURRENT_SOURCE_DIR}/private -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-openbsd/android/include -I${CMAKE_CURRENT_SOURCE_DIR}/upstream-openbsd/lib/libc/include -include openbsd-compat.h")

add_library(c_bionic
  STATIC
  ${libc_bionic_src_files}
  ${libc_bionic_src_files_x86_64}
  )
# Clang assembler has problem with ssse3-strcmp-slm.S, http://b/17302991
set_target_properties(c_bionic PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -no-integrated-as")

add_library(c_common
  STATIC
  ${libc_common_src_files}
  )
set_target_properties(c_common PROPERTIES COMPILE_FLAGS "${libc_common_cflags}")

add_library(c_malloc
  STATIC
  ${libc_malloc_src}
  )
set_target_properties(c_malloc PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -fvisibility=hidden")

add_library(c_stuff
  STATIC
  ${libc_arch_static_src_files}
  ${libc_static_common_src_files}
  bionic/malloc_debug_common.cpp
  bionic/libc_init_static.cpp
  )
set_target_properties(c_stuff PROPERTIES COMPILE_FLAGS "${libc_common_cflags} -DLIBC_STATIC")

add_custom_target(libc.a ALL
  COMMAND ${CMAKE_COMMAND} -E remove -f libc.a
  COMMAND ${CMAKE_AR} crsT libc.a $<TARGET_FILE:c_common> $<TARGET_FILE:c_freebsd> $<TARGET_FILE:c_openbsd> $<TARGET_FILE:c_netbsd> $<TARGET_FILE:c_tzcode> $<TARGET_FILE:c_gdtoa> $<TARGET_FILE:c_bionic> $<TARGET_FILE:c_common> $<TARGET_FILE:c_malloc> $<TARGET_FILE:c_stuff> $<TARGET_FILE:c_syscalls> $<TARGET_FILE:c_cxa>
  COMMENT "Linking CXX static library libc.a"
  )

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/libc.a DESTINATION lib)
install(DIRECTORY include/ DESTINATION include)
install(DIRECTORY arch-x86_64/include/ DESTINATION include)
install(DIRECTORY kernel/uapi/ DESTINATION include)
install(DIRECTORY kernel/uapi/asm-x86/ DESTINATION include)

add_custom_command(OUTPUT crtbrand.s
  COMMAND ${CMAKE_C_COMPILER} -S -o crtbrand.s ${CMAKE_CURRENT_SOURCE_DIR}/bionic/crtbrand.c -DPLATFORM_SDK_VERSION=22
  COMMAND sed -i -e '/\.note\.ABI-tag/s/progbits/note/' crtbrand.s
  DEPENDS bionic/crtbrand.c
  )
add_custom_command(OUTPUT crtbrand.o
  COMMAND ${CMAKE_C_COMPILER} -o crtbrand.o -c crtbrand.s
  DEPENDS crtbrand.s
  )
add_custom_command(OUTPUT crtbegin_static1.o
  COMMAND ${CMAKE_C_COMPILER} -o crtbegin_static1.o -c ${CMAKE_CURRENT_SOURCE_DIR}/arch-common/bionic/crtbegin.c -I${CMAKE_CURRENT_SOURCE_DIR}/include
  DEPENDS arch-common/bionic/crtbegin.c
  )
add_custom_command(OUTPUT crtbegin_static.o
  COMMAND ld -r -o crtbegin_static.o crtbegin_static1.o crtbrand.o
  DEPENDS crtbegin_static1.o crtbrand.o
  )
add_custom_command(OUTPUT crtend_android.o
  COMMAND ${CMAKE_C_COMPILER} -o crtend_android.o -c ${CMAKE_CURRENT_SOURCE_DIR}/arch-common/bionic/crtend.S
  DEPENDS arch-common/bionic/crtend.S
  )
add_custom_target(crt ALL DEPENDS crtbegin_static.o crtend_android.o)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/crtbegin_static.o ${CMAKE_CURRENT_BINARY_DIR}/crtend_android.o DESTINATION lib)
