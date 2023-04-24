## Project
https://www.xpdfreader.com/download.html

## Description
There exists FPE inImageStream::ImageStream(Stream*, int, int, int)  at xpdf-4.04/xpdf/Stream.cc:370:23
## My test program
pdfimages
## Command and argument
./pdfimages poc-file /dev/null
## ASAN Info
```
AddressSanitizer:DEADLYSIGNAL
=================================================================
==2894348==ERROR: AddressSanitizer: FPE on unknown address 0x000000798907 (pc 0x000000798907 bp 0x0c0c000005a7 sp 0x7ffd7d2a6b50 T0)
    #0 0x798907 in ImageStream::ImageStream(Stream*, int, int, int) /root/target/latest/20230404/xpdf-4.04/xpdf/Stream.cc:370:23
    #1 0x4f8e3c in ImageOutputDev::drawImage(GfxState*, Object*, Stream*, int, int, GfxImageColorMap*, int*, int, int) /root/target/latest/20230404/xpdf-4.04/xpdf/ImageOutputDev.cc:324:18
    #2 0x604d02 in Gfx::doImage(Object*, Stream*, int) /root/target/latest/20230404/xpdf-4.04/xpdf/Gfx.cc:4621:7
    #3 0x5aff8e in Gfx::opXObject(Object*, int) /root/target/latest/20230404/xpdf-4.04/xpdf/Gfx.cc:4104:2
    #4 0x5d7c8b in Gfx::execOp(Object*, Object*, int) /root/target/latest/20230404/xpdf-4.04/xpdf/Gfx.cc:862:3
    #5 0x5d6c68 in Gfx::go(int) /root/target/latest/20230404/xpdf-4.04/xpdf/Gfx.cc:747:12
    #6 0x5d5598 in Gfx::display(Object*, int) /root/target/latest/20230404/xpdf-4.04/xpdf/Gfx.cc:669:3
    #7 0x77583e in Page::displaySlice(OutputDev*, double, double, int, int, int, int, int, int, int, int, int (*)(void*), void*) /root/target/latest/20230404/xpdf-4.04/xpdf/Page.cc:422:10
    #8 0x774d48 in Page::display(OutputDev*, double, double, int, int, int, int, int (*)(void*), void*) /root/target/latest/20230404/xpdf-4.04/xpdf/Page.cc:368:3
    #9 0x7890a1 in PDFDoc::displayPage(OutputDev*, int, double, double, int, int, int, int, int (*)(void*), void*) /root/target/latest/20230404/xpdf-4.04/xpdf/PDFDoc.cc:442:27
    #10 0x7892d8 in PDFDoc::displayPages(OutputDev*, int, int, double, double, int, int, int, int, int (*)(void*), void*) /root/target/latest/20230404/xpdf-4.04/xpdf/PDFDoc.cc:460:5
    #11 0x4fc0e4 in main /root/target/latest/20230404/xpdf-4.04/xpdf/pdfimages.cc:156:10
    #12 0x7f4cb41b0082 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x24082)
    #13 0x44b69d in _start (/root/target/latest/20230404/xpdf-4.04/install_map16/bin/pdfimages+0x44b69d)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: FPE /root/target/latest/20230404/xpdf-4.04/xpdf/Stream.cc:370:23 in ImageStream::ImageStream(Stream*, int, int, int)
==2894348==ABORTING
```
## GDB analyze
![image](https://user-images.githubusercontent.com/56296073/233991750-3e58d70d-7de7-4cdc-a074-41b185709df9.png)
It's a division-by-zero error.
## Source Code
```
ImageStream::ImageStream(Stream *strA, int widthA, int nCompsA, int nBitsA) {
  int imgLineSize;

  str = strA;
  width = widthA;
  nComps = nCompsA;
  nBits = nBitsA;

  nVals = width * nComps;
  inputLineSize = (nVals * nBits + 7) >> 3;
  if (width > INT_MAX / nComps ||
      nVals > (INT_MAX - 7) / nBits) {
    // force a call to gmallocn(-1,...), which will throw an exception
    inputLineSize = -1;
  }
  ......
Omit remaining code
  ......
}
  ```
## Version
xpdf:4.04
## POC File
[poc-file.zip](https://github.com/DaisyPo/fuzzing-vulncollect/files/11310114/poc-file.zip)

