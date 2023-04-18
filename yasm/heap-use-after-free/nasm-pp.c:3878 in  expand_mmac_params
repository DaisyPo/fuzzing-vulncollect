# There exists heap-use-after-free in yasm/modules/preprocs/nasm/nasm-pp.c:3878 in expand_mmac_params
## Project：
https://github.com/yasm/yasm

## asan info:
```
==708699==ERROR: AddressSanitizer: heap-use-after-free on address 0x60e0000012a8 at pc 0x55647c385147 bp 0x7ffe09f5d870 sp 0x7ffe09f5d860
READ of size 8 at 0x60e0000012a8 thread T0
    #0 0x55647c385146 in expand_mmac_params modules/preprocs/nasm/nasm-pp.c:3878
    #1 0x55647c38d436 in pp_getline modules/preprocs/nasm/nasm-pp.c:5078
    #2 0x55647c36ac61 in nasm_preproc_get_line modules/preprocs/nasm/nasm-preproc.c:198
    #3 0x55647c35f4ed in nasm_parser_parse modules/parsers/nasm/nasm-parse.c:219
    #4 0x55647c35df6c in nasm_do_parse modules/parsers/nasm/nasm-parser.c:66
    #5 0x55647c35e109 in nasm_parser_do_parse modules/parsers/nasm/nasm-parser.c:83
    #6 0x55647c2f64d4 in do_assemble frontends/yasm/yasm.c:521
    #7 0x55647c2f7281 in main frontends/yasm/yasm.c:753
    #8 0x7fc63b1af082 in __libc_start_main ../csu/libc-start.c:308
    #9 0x55647c2f4b9d in _start (/root/target/yasm/build_asan/bin/yasm+0xa5b9d)

0x60e0000012a8 is located 8 bytes inside of 160-byte region [0x60e0000012a0,0x60e000001340)
freed by thread T0 here:
    #0 0x7fc63b48a40f in __interceptor_free ../../../../src/libsanitizer/asan/asan_malloc_linux.cc:122
    #1 0x55647c331974 in def_xfree libyasm/xmalloc.c:113
    #2 0x55647c370a59 in free_mmacro modules/preprocs/nasm/nasm-pp.c:1163
    #3 0x55647c38cc0a in pp_getline modules/preprocs/nasm/nasm-pp.c:5009
    #4 0x55647c36ac61 in nasm_preproc_get_line modules/preprocs/nasm/nasm-preproc.c:198
    #5 0x55647c35f4ed in nasm_parser_parse modules/parsers/nasm/nasm-parse.c:219
    #6 0x55647c35df6c in nasm_do_parse modules/parsers/nasm/nasm-parser.c:66
    #7 0x55647c35e109 in nasm_parser_do_parse modules/parsers/nasm/nasm-parser.c:83
    #8 0x55647c2f64d4 in do_assemble frontends/yasm/yasm.c:521
    #9 0x55647c2f7281 in main frontends/yasm/yasm.c:753
    #10 0x7fc63b1af082 in __libc_start_main ../csu/libc-start.c:308

previously allocated by thread T0 here:
    #0 0x7fc63b48a808 in __interceptor_malloc ../../../../src/libsanitizer/asan/asan_malloc_linux.cc:144
    #1 0x55647c331857 in def_xmalloc libyasm/xmalloc.c:69
    #2 0x55647c37ffc8 in do_directive modules/preprocs/nasm/nasm-pp.c:3211
    #3 0x55647c38d446 in pp_getline modules/preprocs/nasm/nasm-pp.c:5083
    #4 0x55647c36ac61 in nasm_preproc_get_line modules/preprocs/nasm/nasm-preproc.c:198
    #5 0x55647c35f4ed in nasm_parser_parse modules/parsers/nasm/nasm-parse.c:219
    #6 0x55647c35df6c in nasm_do_parse modules/parsers/nasm/nasm-parser.c:66
    #7 0x55647c35e109 in nasm_parser_do_parse modules/parsers/nasm/nasm-parser.c:83
    #8 0x55647c2f64d4 in do_assemble frontends/yasm/yasm.c:521
    #9 0x55647c2f7281 in main frontends/yasm/yasm.c:753
    #10 0x7fc63b1af082 in __libc_start_main ../csu/libc-start.c:308

SUMMARY: AddressSanitizer: heap-use-after-free modules/preprocs/nasm/nasm-pp.c:3878 in expand_mmac_params
Shadow bytes around the buggy address:
  0x0c1c7fff8200: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1c7fff8210: 00 00 00 00 fa fa fa fa fa fa fa fa 00 00 00 00
  0x0c1c7fff8220: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1c7fff8230: fa fa fa fa fa fa fa fa fd fd fd fd fd fd fd fd
  0x0c1c7fff8240: fd fd fd fd fd fd fd fd fd fd fd fd fa fa fa fa
=>0x0c1c7fff8250: fa fa fa fa fd[fd]fd fd fd fd fd fd fd fd fd fd
  0x0c1c7fff8260: fd fd fd fd fd fd fd fd fa fa fa fa fa fa fa fa
  0x0c1c7fff8270: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c1c7fff8280: fd fd fd fd fa fa fa fa fa fa fa fa fd fd fd fd
  0x0c1c7fff8290: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c1c7fff82a0: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
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
==708699==ABORTING
```
## Command Input:
./yasm poc-file 

[poc-file.zip](https://github.com/yasm/yasm/files/11253102/poc-file.zip) 

poc-file is attached.
## Environment:
OS: Ubuntu 20.04.1
yasm: 1.3.0.55.g101bc (git clone git@github.com:yasm/yasm.git , and compile it)
compile yasm with asan:
./autogen.sh
make distclean
./configure --prefix=$PWD/build_asan
make CC=gcc CXX=g++ CFLAGS="-fsanitize=address -fno-omit-frame-pointer -g" CXXFLAGS="-fsanitize=address -fno-omit-frame-pointer -g"
make install
