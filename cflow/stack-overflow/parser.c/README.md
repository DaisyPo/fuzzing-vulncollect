## Project
https://www.gnu.org/software/cflow/

## Description
There exists stack-overflow because of recursive call of void func_body() and void parse_variable_declaration(Ident *ident, int parm) at cflow-1.7/src/parser.c
## Environment
OS: Ubuntu 20.04.1

Release: cflow 1.7(cflow-latest)

https://ftp.gnu.org/gnu/cflow/cflow-latest.tar.gz

Program:cflow

To reproduce the problem, we need to build cflow with asan:
```
CFLAGS="-fsanitize=address -g -O0" CXXFLAGS="-fsanitize=address -g -O0" \
  ./configure
  make
  make install
```
## Command and argument
./cflow -o /dev/null poc-file

poc-file is attached below.

## ASAN Info
```
AddressSanitizer:DEADLYSIGNAL
=================================================================
==2158725==ERROR: AddressSanitizer: stack-overflow on address 0x7ffe379ebf88 (pc 0x00000042fb34 bp 0x7ffe379ec7f0 sp 0x7ffe379ebf90 T0)
    #0 0x42fb34 in strcmp /root/llvm-project-llvmorg-10.0.1/compiler-rt/lib/asan/../sanitizer_common/sanitizer_common_interceptors.inc:451:3
    #1 0x4efce2 in hash_symbol_compare /root/target/latest/20230404/cflow-1.7/src/symbol.c:63:35
    #2 0x521e4f in hash_lookup /root/target/latest/20230404/cflow-1.7/gnu/hash.c:252:34
    #3 0x4ef08f in lookup /root/target/latest/20230404/cflow-1.7/src/symbol.c:76:11
    #4 0x4d50e2 in ident /root/target/latest/20230404/cflow-1.7/src/c.l:264:24
    #5 0x4cc1b2 in yylex /root/target/latest/20230404/cflow-1.7/src/c.l:124:8
    #6 0x4d6a26 in get_token /root/target/latest/20230404/cflow-1.7/src/c.l:380:17
    #7 0x4e47c0 in nexttoken /root/target/latest/20230404/cflow-1.7/src/parser.c:298:11
    #8 0x4eb4d9 in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1069:4
    #9 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #10 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #11 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #12 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #13 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #14 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #15 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #16 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #17 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #18 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #19 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #20 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #21 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #22 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #23 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #24 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #25 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #26 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #27 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #28 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #29 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #30 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #31 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #32 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #33 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #34 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #35 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #36 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #37 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #38 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #39 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #40 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #41 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #42 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #43 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #44 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #45 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #46 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #47 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #48 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #49 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #50 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #51 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #52 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #53 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #54 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #55 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #56 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #57 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #58 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #59 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #60 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #61 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #62 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #63 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #64 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #65 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #66 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #67 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #68 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #69 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #70 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #71 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #72 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #73 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #74 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #75 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #76 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #77 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #78 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #79 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #80 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #81 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #82 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #83 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #84 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #85 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #86 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #87 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #88 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #89 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #90 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #91 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #92 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #93 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #94 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #95 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #96 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #97 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #98 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #99 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #100 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #101 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #102 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #103 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #104 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #105 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #106 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #107 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #108 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #109 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #110 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #111 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #112 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #113 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #114 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #115 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #116 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #117 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #118 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #119 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #120 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #121 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #122 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #123 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #124 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #125 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #126 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #127 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #128 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #129 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #130 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #131 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #132 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #133 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #134 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #135 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #136 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #137 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #138 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #139 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #140 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #141 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #142 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #143 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #144 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #145 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #146 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #147 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #148 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #149 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #150 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #151 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #152 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #153 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #154 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #155 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #156 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #157 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #158 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #159 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #160 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #161 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #162 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #163 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #164 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #165 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #166 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #167 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #168 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #169 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #170 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #171 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #172 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #173 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #174 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #175 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #176 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #177 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #178 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #179 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #180 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #181 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #182 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #183 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #184 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #185 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #186 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #187 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #188 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #189 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #190 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #191 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #192 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #193 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #194 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #195 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #196 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #197 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #198 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #199 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #200 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #201 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #202 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #203 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #204 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #205 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #206 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #207 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #208 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #209 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #210 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #211 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #212 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #213 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #214 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #215 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #216 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #217 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #218 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #219 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #220 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #221 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #222 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #223 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #224 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #225 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #226 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #227 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #228 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #229 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #230 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #231 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #232 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #233 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #234 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #235 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #236 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #237 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #238 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #239 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #240 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #241 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #242 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #243 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #244 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #245 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #246 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9
    #247 0x4e7ead in parse_variable_declaration /root/target/latest/20230404/cflow-1.7/src/parser.c:801:4
    #248 0x4eb65a in func_body /root/target/latest/20230404/cflow-1.7/src/parser.c:1082:9

SUMMARY: AddressSanitizer: stack-overflow /root/llvm-project-llvmorg-10.0.1/compiler-rt/lib/asan/../sanitizer_common/sanitizer_common_interceptors.inc:451:3 in strcmp
==2158725==ABORTING
```


## POC File
[poc-file.zip](https://github.com/DaisyPo/fuzzing-vulncollect/files/11343936/poc-file.zip)
