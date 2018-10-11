<!--BadgesSTART-->
[![Read Me Synchronizer](https://img.shields.io/badge/-powered%20by%20read%20me%20synchronizer-brightgreen.svg)](https://github.com/undefined/ReadMeSynchronizer)
<!-- Powered by https://github.com/undefined/ReadMeSynchronizer -->

[Subscribe](https://github.com/GregTrevellick/awib/subscription) to receive notificatons.

[![BetterCodeHub compliance](https://bettercodehub.com/edge/badge/GregTrevellick/awib?branch=master)](https://bettercodehub.com/results/GregTrevellick/awib)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/9db6094d5fd342ee8a8740efd33526c9)](https://www.codacy.com/project/gtrevellick/awib/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=GregTrevellick/awib&amp;utm_campaign=Badge_Grade_Dashboard)
[![CodeFactor](https://www.codefactor.io/repository/github/GregTrevellick/awib/badge)](https://www.codefactor.io/repository/github/GregTrevellick/awib)
[![GitHub top language](https://img.shields.io/github/languages/top/GregTrevellick/awib.svg)](https://github.com/GregTrevellick/awib)
[![Github language count](https://img.shields.io/github/languages/count/GregTrevellick/awib.svg)](https://github.com/GregTrevellick/awib)
[![GitHub issues](https://img.shields.io/github/issues-raw/GregTrevellick/awib.svg)](https://github.com/GregTrevellick/awib/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr-raw/GregTrevellick/awib.svg)](https://github.com/GregTrevellick/awib/pulls)














[![Hound](https://img.shields.io/badge/hound_ci-checked-brightgreen.svg)](https://houndci.com/)
[![Access Lint github](https://img.shields.io/badge/a11y-checked-brightgreen.svg)](https://www.accesslint.com)
[![ImgBot](https://img.shields.io/badge/images-optimized-brightgreen.svg)](https://imgbot.net/)
[![Renovate Bot github](https://img.shields.io/badge/renovatebot-checked-brightgreen.svg)](https://renovatebot.com/)
[![Charity Ware](https://img.shields.io/badge/charity%20ware-thank%20you-brightgreen.svg)](https://github.com/GregTrevellick/MiscellaneousArtefacts/wiki/Charity-Ware)
[![License](https://img.shields.io/github/license/gittools/gitlink.svg)](/LICENSE.txt)
<!--BadgesEND-->


Awib is a brainfuck compiler entirely written in brainfuck.

- Awib implements several optimization strategies and its compiled
  output outperforms that of many other brainfuck compilers
- Awib is itself a 4-language polyglot and can be run/compiled
  as brainfuck, Tcl, C and bash
- Awib has 6 separate backends and is capable of
  compiling brainfuck source code to Linux executables (i386) and
  five programming languages: C, Tcl, Go, Ruby and Java

Usage
-----

Feed awib brainfuck source code as input and the compiled program
will be written as output.

Awib is a cross-compiler. The supported target platforms are
listed below. By default, the target "lang_c" is chosen.

To specify a target platform, insert a line on the form "@TARGET"
(without the quotation marks and with "TARGET" suitably replaced)
at the very beginning of the source code you wish to compile.
Awib will then produce output accordingly.

    386_linux - Linux executables for i386
    lang_c    - C code
    lang_ruby - Ruby code
    lang_go   - Go code
    lang_tcl  - Tcl code
    lang_java - Java code
For instance, the following input would produce an executable hello
world-program for Linux:

    @386_linux
    ++++++[->++++++++++++<]>.----[--<+++>]<-.+++++++..+++.[--->+<]>-----.--
    -[-<+++>]<.---[--->++++<]>-.+++.------.--------.-[---<+>]<.[--->+<]>-.

The following would produce a hello world-program in Ruby:

    @lang_ruby
    ++++++[->++++++++++++<]>.----[--<+++>]<-.+++++++..+++.[--->+<]>-----.--
    -[-<+++>]<.---[--->++++<]>-.+++.------.--------.-[---<+>]<.[--->+<]>-.

And this file would give you the hello world-program in C:

    @lang_c
    ++++++[->++++++++++++<]>.----[--<+++>]<-.+++++++..+++.[--->+<]>-----.--
    -[-<+++>]<.---[--->++++<]>-.+++.------.--------.-[---<+>]<.[--->+<]>-.


Optimizations
-------------

Awib is an optimizing compiler:

-  Sequences of '-','>','<' and '+' are contracted into single
   instructions. E.g. "----" is replaced with a single SUB(4).
-  Mutually cancelling instructions are reduced. E.g. "+++-->><"
   is equivalent to "+>" and is compiled accordingly.
-  Some common constructs are identified and replaced with single
   instructions. E.g. "[-]" is compiled into a single SET(0).
-  Loops known to never be entered are removed. This is the case
   for loops opened at the very beginning of a program (when all
   cells are 0) and loops opened immediately after the closing
   of another loop.
- Copy and multiplication loops are replaced with constant time
  operations. E.g. "[->>+++<+<]" is compiled into two RMUL(2, 3),
  RMUL(1,1)), SET(0)..

Requirements
------------

Awib will run smoothly in any brainfuck environment where:

-  Cells are 8-bit or larger
-  The read instruction ',' (comma) issued after end of
   input results in 0 being written OR -1 being written
   OR no change being made to the cell at all.

The vast majority of brainfuck environments meet these criteria.

Since awib is polyglot, it is also possible to compile and/or run awib
directly as C, tcl or bash. For instance, using gcc, the following
will build an executable file called awib from awib-0.2.b.

    $ cp awib-0.2.b awib-0.2.c
    $ gcc awib-0.2.c -o awib.tmp
    $ ./awib.tmp < awib-0.2.b > awib-0.2.c
    $ gcc -O2 awib-0.2.c -o awib

Using bash works fine, but is very very very slow:

    $ (echo "@386_linux"; cat awib.b) | bash awib.b > awib
    $ chmod +x awib

And tcl:

    $ (echo "@386_linux"; cat awib.b) | tclsh awib.b > awib
    $ chmod +x awib


Environment
-----------

Code compiled with awib will execute in an environment where:

-  Cells are 8-bit wrapping integers.
-  Issuing the read instruction ',' (comma) after
   end of input results in the current cell being
   left as is  (no-change on EOF).
-  At least 2^16-1 = 65535 cells are available.
-  Operating beyond the available memory, in either
   direction, results in undefined behaviour.


License
-------

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.


Mats Linander, 2014-09-14
