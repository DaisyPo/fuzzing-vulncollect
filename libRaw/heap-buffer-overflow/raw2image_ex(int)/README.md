## Project
https://github.com/LibRaw/LibRaw
## Description
There exists heap-buffer-overflow in LibRaw::raw2image_ex(int) src/preprocessing/raw2image.cpp:492
## My test program
dcraw_half in  Libraw/bin
## Command and argument
./dcraw_half crash_dcraw_half  
## Crash Information
The output of exampletest with address sanitizer enabled
```
==611348==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x62d000013300 at pc 0x7fc7701d804d bp 0x7ffc1d184d60 sp 0x7ffc1d184508
WRITE of size 2224 at 0x62d000013300 thread T0
    #0 0x7fc7701d804c in __interceptor_memmove ../../../../src/libsanitizer/sanitizer_common/sanitizer_common_interceptors.inc:773
    #1 0x7fc770001f0e in LibRaw::raw2image_ex(int) src/preprocessing/raw2image.cpp:492
    #2 0x7fc76ffd2bd3 in LibRaw::dcraw_process() src/postprocessing/dcraw_process.cpp:43
    #3 0x56227007b71b in main samples/dcraw_half.c:66
    #4 0x7fc76f814082 in __libc_start_main ../csu/libc-start.c:308
    #5 0x56227007be0d in _start (/root/target/latest/LibRaw-0.21.1/build_afl_asan/bin/dcraw_half+0x2e0d)

0x62d000013300 is located 0 bytes to the right of 36608-byte region [0x62d00000a400,0x62d000013300)
allocated by thread T0 here:
    #0 0x7fc770244a06 in __interceptor_calloc ../../../../src/libsanitizer/asan/asan_malloc_linux.cc:153
    #1 0x7fc77004cca5 in libraw_memmgr::calloc(unsigned long, unsigned long) libraw/libraw_alloc.h:56
    #2 0x7fc77004cca5 in LibRaw::calloc(unsigned long, unsigned long) src/utils/utils_libraw.cpp:271

SUMMARY: AddressSanitizer: heap-buffer-overflow ../../../../src/libsanitizer/sanitizer_common/sanitizer_common_interceptors.inc:773 in __interceptor_memmove
Shadow bytes around the buggy address:
  0x0c5a7fffa610: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c5a7fffa620: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c5a7fffa630: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c5a7fffa640: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c5a7fffa650: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c5a7fffa660:[fa]fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c5a7fffa670: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c5a7fffa680: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c5a7fffa690: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c5a7fffa6a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c5a7fffa6b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==611348==ABORTING
```
## Version
LibRaw 0.21.1 release
the commit is cccb976
## POC File

[crash_dcraw_half.zip](https://github.com/LibRaw/LibRaw/files/10416372/crash_dcraw_half.zip)

