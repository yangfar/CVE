### Description
Some crashes occurred in function read_pointer at binbloom-master/src/helpers.c:67:24 when running program binbloom, this can reproduce on the latest commit.
### Version
Binbloom 2.0
latest commit[https://github.com/quarkslab/binbloom/commit/b9aada98fa98924d7d3d90e638e865df9f9a2e53](url)
Linux 5.15.0-52-generic #58~20.04.1-Ubuntu SMP Thu Oct 13 13:09:46 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
### Command
./binbloom ./POC
### Crashe
==487329==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x631000010f90 at pc 0x0000004d3963 bp 0x7ffece9f22d0 sp 0x7ffece9f22c8
READ of size 4 at 0x631000010f90 thread T0
    #0 0x4d3962 in read_pointer /home/hjsz/fuzz_software/binbloom-master/src/helpers.c:67:24
    #1 0x4cc0e5 in compute_candidates /home/hjsz/fuzz_software/binbloom-master/src/binbloom.c:1134:21
    #2 0x4d0131 in find_base_address /home/hjsz/fuzz_software/binbloom-master/src/binbloom.c
    #3 0x4d2127 in main /home/hjsz/fuzz_software/binbloom-master/src/binbloom.c:2102:17
    #4 0x7f9ffd152082 in __libc_start_main /build/glibc-SzIz7B/glibc-2.31/csu/../csu/libc-start.c:308:16
    #5 0x41c3ed in _start (/home/hjsz/fuzz_software/binbloom-master/src/binbloom+0x41c3ed)

0x631000010f91 is located 0 bytes to the right of 67473-byte region [0x631000000800,0x631000010f91)
allocated by thread T0 here:
    #0 0x494b2d in malloc (/home/hjsz/fuzz_software/binbloom-master/src/binbloom+0x494b2d)
    #1 0x4cfd95 in find_base_address /home/hjsz/fuzz_software/binbloom-master/src/binbloom.c:1655:39
    #2 0x7f9ffd152082 in __libc_start_main /build/glibc-SzIz7B/glibc-2.31/csu/../csu/libc-start.c:308:16

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/hjsz/fuzz_software/binbloom-master/src/helpers.c:67:24 in read_pointer
Shadow bytes around the buggy address:
  0x0c627fffa1a0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627fffa1b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627fffa1c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627fffa1d0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c627fffa1e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c627fffa1f0: 00 00[01]fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627fffa200: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627fffa210: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627fffa220: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627fffa230: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627fffa240: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
  Shadow gap:              cc
==487329==ABORTING
### Crashes and POC
[POC.zip](https://github.com/quarkslab/binbloom/files/9915006/POC.zip)
[Crashes.zip](https://github.com/quarkslab/binbloom/files/9915011/Crashes.zip)