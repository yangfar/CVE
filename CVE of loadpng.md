# CVE of loadpng

## SEGV on unknown address

### Version
pngdetail by Lode Vandevenne
version: 20220717
### Command
./pngdetail @@
### Crash Output
##### AddressSanitizer:DEADLYSIGNAL
=================================================================

==2262494==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000003 (pc 0x0000004f43b4 bp 0x000000000080 sp 0x7ffd35c4f320 T0)

==2262494==The signal is caused by a WRITE memory access.

==2262494==Hint: address points to the zero page.

    #0 0x4f43b4 in readChunk_tRNS(LodePNGColorMode*, unsigned char const*, unsigned long) /home/hjsz/fuzz_software/lodepng-master/lodepng.cpp:4406:65
    
    #1 0x4f2716 in lodepng_inspect_chunk(LodePNGState*, unsigned long, unsigned char const*, unsigned long) /home/hjsz/fuzz_software/lodepng-master/lodepng.cpp:4793:13
    #2 0x5a39c0 in inspect_chunk_by_name(unsigned char const*, unsigned char const*, lodepng::State&, char const*) /home/hjsz/fuzz_software/lodepng-master/pngdetail.cpp:155:10
    
    #3 0x5a39c0 in Data::loadInspect() /home/hjsz/fuzz_software/lodepng-master/pngdetail.cpp:221:7
    
    #4 0x591e19 in showHeaderInfo(Data&, Options const&) /home/hjsz/fuzz_software/lodepng-master/pngdetail.cpp:1109:8
    
    #5 0x59db24 in showInfos(Data&, Options const&) /home/hjsz/fuzz_software/lodepng-master/pngdetail.cpp:1330:79
    
    #6 0x5a12a6 in main /home/hjsz/fuzz_software/lodepng-master/pngdetail.cpp:1444:5
    
    #7 0x7fd0de890082 in __libc_start_main /build/glibc-SzIz7B/glibc-2.31/csu/../csu/libc-start.c:308:16
    
    #8 0x41d76d in _start (/home/hjsz/fuzz_software/lodepng-master/pngdetail+0x41d76d)

AddressSanitizer can not provide additional info.

SUMMARY: AddressSanitizer: SEGV /home/hjsz/fuzz_software/lodepng-master/lodepng.cpp:4406:65 in readChunk_tRNS(LodePNGColorMode*, unsigned char const*, unsigned long)
==2262494==ABORTING

### POC

[POC.zip](https://github.com/lvandeve/lodepng/files/9843228/POC.zip)

**Report of the Information Security Laboratory of Ocean University of China @OUC_ISLOUC @OUC_Blue_Whale**

Thanks for your time!



## Memory Leak

### Detail
=================================================================
==1082665==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 16384 byte(s) in 4 object(s) allocated from:
    #0 0x495dcd in malloc (/home/hjsz/fuzz_software/lodepng-master/benchmark+0x495dcd)
    #1 0x4fee62 in lodepng_malloc(unsigned long) /home/hjsz/fuzz_software/lodepng-master/lodepng.cpp:78:10
    #2 0x4fee62 in lodepng_decode(unsigned char**, unsigned int*, unsigned int*, LodePNGState*, unsigned char const*, unsigned long) /home/hjsz/fuzz_software/lodepng-master/lodepng.cpp:5055:28
    #3 0x52ecfc in testDecode(std::vector<unsigned char, std::allocator<unsigned char> > const&) /home/hjsz/fuzz_software/lodepng-master/lodepng_benchmark.cpp:197:26
    #4 0x5318a5 in testFile(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&) /home/hjsz/fuzz_software/lodepng-master/lodepng_benchmark.cpp:261:5
    #5 0x533b28 in main /home/hjsz/fuzz_software/lodepng-master/lodepng_benchmark.cpp:312:5
    #6 0x7f6fd43cf082 in __libc_start_main /build/glibc-SzIz7B/glibc-2.31/csu/../csu/libc-start.c:308:16

SUMMARY: AddressSanitizer: 16384 byte(s) leaked in 4 allocation(s).
**Thanks for your time.**

I saw the issues #165 , It should be the same problem. If the crash occues when normal user use the function rather than fuzzing the function?
