﻿Meriken's Tripcode Engine
=========================

This repository was moved from https://github.com/meriken/merikens-tripcode-engine

"Meriken's Tripcode Engine" is a cross-platform application designed to generate custom/vanity tripcodes at maximum speed. 
It is arguably the fastest and most powerful program of its kind. It makes effecitive use of available computing power of CPUs and GPUs, 
and the user can specify flexible regex patterns for desired tripcodes. It features highly optimized, extensively parallelized 
implementations of bitslice DES and SHA-1 for OpenCL, AMD GCN, NVIDIA CUDA, and Intel SSE2/AVX/AVX2.

## Downloads

Stable versions are available at the following links:

### Precompiled Binaries for Windows

* [MerikensTripcodeEngine_3.2.0_English_Windows.zip]( https://goo.gl/3b2kT6 )
* [MerikensTripcodeEngine_2.1.2_English.zip]( http://j.mp/26rh3x7 )

### Source Codes

* [v3.2.0.tar.gz]( https://goo.gl/otcMpi )

## Performance

Here are some actual speeds the author achieved with this tripcode generator:

* AMD Radeon HD 7990 **1022MH/s** (descrypt; 1250mV +20% 1180MHz)
* NVIDIA GeForce GTX 980 Ti **996MH/s** (descrypt; 110% +250MHz)
* AMD Radeon HD 290X **647M tripcode/s** (descrypt; +100mV +50% 1074MHz)
* AMD Radeon HD 7970 **408M tripcode/s** (descrypt; 1000MHz)

Currently [MTY CL][2] is the only practical alternative to this program, and this program runs much faster than MTY CL in most cases:

```
Hardware and Software Configuration:
OS: Microsoft Windows 7 SP1 Professional
CPU: Intel Core i7-3770K @ 3.5GHz
GPU: Gigabyte Radeon HD 7970 @ 1060MHz
Display Driver: AND Catalyst 15.7.1
Target Pattern: ^TEST//

Results:
Tripcode Explorer: 16M tripcode/s
MTY CL 0.52: 285M tripcode/s
Meriken's Tripcode Engine 2.0.6: 427M tripcode/s
```

[2]: https://github.com/madsbuvi/MTY_CL

## Donations

I would really appreciate donations as it is quite expensive to keep buying hardware for testing.
I am also working on the English version of my tripcode search service and would like to add more servers.

* PayPal: `meriken.ygch.net@gmail.com`
* Bitcoin: `1BZrWADRhLr9DyQYYRJhRcmudE3vntT5em`

## Windows

### Building

You need the following tools to build Meriken's Tripcode Engine.

* Visual Studio 2013 Community
* CUDA Toolkit 7.5
* AMD APP SDK 3.0
* YASM **1.2.0** (Do not use YASM 1.3.0!)

This program uses Boost 1.61.0 and Boost.Process 0.5. Make sure to extract `BoostPackages/boost_1_61_0.7z` and run `BoostPackages/BuildBoostForVisualStudio.bat` before building `VisualStudio/MerikensTripcodeEngine.sln`.

There are several configurations. If you are using a 64-bit operating system, you need to build both 32-bit and 64-bit executables. Please note that NVIDIA-optimized versions take **extremely** long time to build.

### Dependencies

You need the following software installed in order to run the application:

* [AMD Radeon Desktop Video Card Driver][4]
  (if you are using an AMD graphics card)
* [NVIDIA Display Driver Version 352.78 or later][5]
  (if you are using an NVIDIA graphics card)

[4]: http://support.amd.com/en-us/download
[5]: http://www.nvidia.com/Download/index.aspx?lang=en-us

### Usage

Specify search patterns in `patterns.txt` and run either
`MerikensTripcodeEngine.exe`, if you are using a 32-bit operating system, or
`MerikensTripcodeEngine64.exe`, if you are using a 64-bit operating system.
Matching tripcodes will be displayed and saved in `tripcodes.txt`. See "Example of 'patterns.txt'" and "Options" below.

## Linux and Other POSIX-Compliant Operating Systems

### Building

You should be able to build and run this application on any POSIX-compliant operating systems. You need the following tools to build Meriken's Tripcode Engine.

* CMake 2.8.4 or later
* C++11-compliant compiler (g++-4.8 or later/clang++-3.5 or later)
* AMD APP SDK 3.0 (if you are using an AMD video card.)
* CUDA Toolkit 7.5 (if you are using an NVIDIA video card.)

You should be able to build everything with `BuildAll.sh`. If you are building the application manually, make sure to extract `BoostPackages/boost_1_61_0.7z` and build it before building `MerikensTripcodeEngine`. You also need to build and install CLRadeonExtender in the package.

**Note:** AVX2 is not supported on POSIX-compliant operating systems.  

#### Build Instructions for Ubuntu 14.04 LTS/16.04 LTS

```
$ sudo apt-get update && sudo apt-get install p7zip-full libbz2-dev python2.7-dev 
$ ./BuildAll.sh
$ sudo make -C CLRadeonExtender/CLRX-mirror-master/build install
$ sudo make -C CMakeBuild install
```

If you would like to use an AMD graphics card, you also need to run `sudo apt install fglrx-updates fglrx-updates-dev` and install [AMD APP SDK 3.0]( http://developer.amd.com/tools-and-sdks/opencl-zone/amd-accelerated-parallel-processing-app-sdk/ ) before building `MerikensTripcodeEngine`.

For an NVIDIA graphics card, you also need to install [CUDA Toolkit 7.5]( https://developer.nvidia.com/cuda-toolkit ) before building `MerikensTripcodeEngine`.

You can reconfigure the application after you run `BuildAll.sh`:

```
$ cd CMakeBuild
$ cmake -DENABLE_OPENCL=ON -DENABLE_CUDA=ON -DENGLISH_VERSION=ON -DENABLE_CUDA_DES_MULTIPLE_KERNELS_MODE=OFF ..
$ make clean && make -j 8
$ sudo make install
```

Please note that NVIDIA-optimized versions (-DENABLE_CUDA_DES_MULTIPLE_KERNELS_MODE=ON) take **extremely** long time to build.

### Usage

Specify search patterns in `patterns.txt` and run `MerikensTripcodeEngine`.
Matching tripcodes will be displayed and saved in `tripcodes.txt`.
See "Example of 'patterns.txt'" and "Options" below.

## Example of "patterns.txt"

```
# Meriken's Tripcode Engine English
# Copyright (c) 2011-2016 !/Meriken/. <meriken.ygch.net@gmail.com>
#
# - Specify only one pattern in each line.
# - Patterns must be at least 5 characters in length.
# - Patterns that are too long will be ignored.
# - Strings after '#' are treated as comments.



# Specify non-regex patterns after the "#noregex" directive.
# You can only use [A-Za-z0-9./] for patterns.

#noregex

TEST/                   # Matches "!TEST/UH3.F", "!TEST/ZXVew", etc.



# Specify regex patterns after the "#regex" directive.
# The following operators and specifiers are available for use:
# 
#     ^ $ () | [] [^] . + * ? \ {n} {m,n} \n
#     [:alpha:] [:upper:] [:lower:] [:digit:] [:alnum:] [:punct:]
# 
# It is encouraged to use '^' whenever possible to achieve maximum
# search speed.

#regex

#^TEST/                 # Matches "!TEST/UH3.F", "!TEST/ZXVew", etc.
#/TEST$                 # Matches "!15ycs/TEST", "!wtra5/TEST", etc.
#/TEST/                 # Matches "!y/TEST/5uj", "!anj/TEST/.", etc.
#^[0-9]*$               # Matches "!8710915015", "!9104552720", etc.
#^([:upper:]{5})\1$     # Matches "!IOPAFIOPAF", "!UIABTUIABT", etc.
#^[Mm]eriken[:punct:]   # Matches "!meriken/u6", "!Meriken.qe", etc.



#ignore
Lines between "#ignore" are "#endignore" will be ignored.
#endignore



# You cannot specify a pattern in the last line.
```

## Options

**-g** : Use GPUs as search devices. (This option can be used in combination with "-c".)

**-d** [device number] : Specify a GPU to use.

**-c** : Use CPUs as search devices. (This option can be used in combination with "-g".)

**-l** [length of tripcodes] : Specify either 10 or 12. (Please note that you can use 12 character tripcodes only at 2ch.net.)

**-x** [number of blocks/SM] : Specify the number of blocks per SM (1 <= n <= 256) for CUDA devices.

**-t** [number of threads] : Specify the number of CPU search threads.

**-o** [output file] : Specify an output file.

**-f** [input file] : Specify an input file.

**--use-one-and-two-byte-characters-for-keys** : Use Shift-JIS characters for keys.

**--disable-gcn-assembler** : Disable GCN assembler and use OpenCL kernels instead.

## Support Threads

I occasionaly create support threads for this program on 4chan to receive direct feedback from its users. The following are archives of the past support threads:

* [Meriken's Tripcode Engine English (9/26/2013)]( http://archive.rebeccablacktech.com/g/thread/37003452 )
* [Meriken's Tripcode Engine English No. 2 (10/2/2013)]( http://archive.rebeccablacktech.com/g/thread/37126997 )
* [Meriken's Tripcode Engine English No. 3 (10/13/2015)]( http://archive.rebeccablacktech.com/g/thread/50803429 ) 
* [Meriken's Tripcode Engine English No. 4 (10/17/2015)]( https://archive.rebeccablacktech.com/g/thread/50871258 )
* [Meriken's Tripcode Engine English No. 5 (4/24/2016)]( https://archive.rebeccablacktech.com/g/thread/54208823 )
* [Meriken's Tripcode Engine English No. 5 (5/14/2016)]( http://boards.4chan.org/g/thread/54553624 )

## Source Code

The source code is hosted on GitHub:

[https://github.com/meriken/merikens-tripcode-engine-v3]( https://github.com/meriken/merikens-tripcode-engine-v3 )
	  
## Miscellaneous Notes

Please feel free to contact the author at [meriken.ygch.net@gmail.com]( mailto:meriken.ygch.net@gmail.com ) for feedback, bug reports, suggestions, etc.

"Meriken's Tripcode Engine" is part of the GUI-based, network-capable [Meriken's Tripcode Generator]( http://meriken.ygch.net/programming/merikens-tripcode-generator/ ), which is intended primarily for users of 2ch.net in Japan. If Japanese does not discourage you, check out the original application as well as [Meriken's Tripcode Yggdrasil]( http://tripcode.ygch.net/yggdrasil/ ), a web-based distributed tripcode generation service.

## License

Meriken's Tripcode Engine is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

Meriken's Tripcode Engine is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with Meriken's Tripcode Engine.  If not, see <http://www.gnu.org/licenses/>.

Copyright © 2016 ◆/Meriken/.
