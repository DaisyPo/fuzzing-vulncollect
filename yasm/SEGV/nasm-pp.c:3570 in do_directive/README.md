# There exists SEGV in yasm/modules/preprocs/nasm/nasm-pp.c:3570 in do_directive 
## Project：
https://github.com/yasm/yasm

## asan info:
```
AddressSanitizer:DEADLYSIGNAL
=================================================================
==1433063==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x55736a1a1cff bp 0x7fff4d5cbf50 sp 0x7fff4d5cba60 T0)
==1433063==The signal is caused by a READ memory access.
==1433063==Hint: address points to the zero page.
    #0 0x55736a1a1cfe in do_directive modules/preprocs/nasm/nasm-pp.c:3570
    #1 0x55736a1ac446 in pp_getline modules/preprocs/nasm/nasm-pp.c:5083
    #2 0x55736a189c61 in nasm_preproc_get_line modules/preprocs/nasm/nasm-preproc.c:198
    #3 0x55736a17e4ed in nasm_parser_parse modules/parsers/nasm/nasm-parse.c:219
    #4 0x55736a17cf6c in nasm_do_parse modules/parsers/nasm/nasm-parser.c:66
    #5 0x55736a17d109 in nasm_parser_do_parse modules/parsers/nasm/nasm-parser.c:83
    #6 0x55736a1154d4 in do_assemble frontends/yasm/yasm.c:521
    #7 0x55736a116281 in main frontends/yasm/yasm.c:753
    #8 0x7fbf8492d082 in __libc_start_main ../csu/libc-start.c:308
    #9 0x55736a113b9d in _start (/root/target/latest/20230404/yasm/build_asan/bin/yasm+0xa5b9d)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV modules/preprocs/nasm/nasm-pp.c:3570 in do_directive
==1433063==ABORTING
```
## Command Input:
./yasm poc-file 

[poc-file.zip](https://github.com/yasm/yasm/files/11253195/poc-file.zip)

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
