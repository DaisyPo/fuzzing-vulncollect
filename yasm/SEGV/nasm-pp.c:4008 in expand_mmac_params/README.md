# There exists SEGV in yasm/modules/preprocs/nasm/nasm-pp.c:4008 in expand_mmac_params
## asan info:
```
AddressSanitizer:DEADLYSIGNAL
=================================================================
==3645333==ERROR: AddressSanitizer: SEGV on unknown address 0x60bcf518f500 (pc 0x562c742f3049 bp 0x7ffc6fb07330 sp 0x7ffc6fb07200 T0)
==3645333==The signal is caused by a READ memory access.
    #0 0x562c742f3048 in expand_mmac_params modules/preprocs/nasm/nasm-pp.c:4008
    #1 0x562c742ead70 in do_directive modules/preprocs/nasm/nasm-pp.c:2950
    #2 0x562c742fa446 in pp_getline modules/preprocs/nasm/nasm-pp.c:5083
    #3 0x562c742d7c61 in nasm_preproc_get_line modules/preprocs/nasm/nasm-preproc.c:198
    #4 0x562c742cc4ed in nasm_parser_parse modules/parsers/nasm/nasm-parse.c:219
    #5 0x562c742caf6c in nasm_do_parse modules/parsers/nasm/nasm-parser.c:66
    #6 0x562c742cb109 in nasm_parser_do_parse modules/parsers/nasm/nasm-parser.c:83
    #7 0x562c742634d4 in do_assemble frontends/yasm/yasm.c:521
    #8 0x562c74264281 in main frontends/yasm/yasm.c:753
    #9 0x7f44d85c0082 in __libc_start_main ../csu/libc-start.c:308
    #10 0x562c74261b9d in _start (/root/target/yasm/build_asan/bin/yasm+0xa5b9d)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV modules/preprocs/nasm/nasm-pp.c:4008 in expand_mmac_params
==3645333==ABORTING
```
## Command Input:
./yasm poc-file 

[poc-file.zip](https://github.com/yasm/yasm/files/11252936/poc-file.zip)

poc-file is attached.

## Environment
OS: Ubuntu 20.04.1
yasm: 1.3.0.55.g101bc (git clone git@github.com:yasm/yasm.git , and compile it)
compile yasm with asan:
./autogen.sh
make distclean
./configure --prefix=$PWD/build_asan
make CC=gcc CXX=g++ CFLAGS="-fsanitize=address -fno-omit-frame-pointer -g" CXXFLAGS="-fsanitize=address -fno-omit-frame-pointer -g"
make install
You can also reproduce it without ASAN , don't set CFLAGS and CXXFLAGS and directly use GDB.

## GDB results:
![image](https://user-images.githubusercontent.com/56296073/232665483-cc9ef453-fc5d-4b43-bc3d-1b248f133a5b.png)

we use "p mac->params[n]"but can't access memory. SEGV happens.

