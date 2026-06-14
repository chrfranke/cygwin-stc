sigsegv = executable('sigsegv', 'sigsegv.c')

# verify basic coredump functionality
coredump_sh = find_program('coredump.sh')
e = environment({'SIGSEGV': sigsegv.full_path()})
test('coredump', coredump_sh, env: e)

# verify that an already attached debugger (i.e. strace) doesn't put us into an infinite loop on segfault
# https://cygwin.com/pipermail/cygwin/2025-May/258144.html
#
# (this test doesn't work reliably (sometimes hanging) under x86_64 emulation on
# arm64; we don't have the hardware to investigate, so skip it)
kern_name = run_command('uname', '-s', check:true)
if kern_name.stdout().contains('ARM64')
  skip = find_program('skip_test.py')
  test('strace-sigsegv', skip)
else
  strace = find_program('strace')
  test('strace-sigsegv', strace, args: sigsegv)
endif
