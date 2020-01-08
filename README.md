[//]: # (-*- coding: utf-8 -*-)

# sfmt-erlang: SIMD-oriented Fast Mersenne Twister (SFMT) for Erlang

* Version 0.13.1 8-JAN-2020
* Edited and written by Kenji Rikitake (Kenji Rikitake Professional Engineer's Office)
* Email contact: <kenji.rikitake@acm.org>

## Security notice regarding the PHP mt_seed() vulnerability

Ambionics Security published [an internal state retrieval algorithm of PHP `mt_rand()`](https://www.ambionics.io/blog/php-mt-rand-prediction) on 6-JAN-2020. sfmt-erlang uses the same seed-to-internal-state initialization algorithm at the function `init_gen_rand/1`.

For reducting the possibility of the internal state revelation, use `init_by_list32/1` instead, better combined with `rand:uniform/1`. [Raimo Niskanen published a piece of code for this purpose](http://erlang.org/pipermail/erlang-questions/2018-July/095875.html).

*Note well that sfmt-erlang has no cryptographic security guarantee and MUST NOT be used for security purposes such as password generation.*

Also: Version 0.13.0 and 0.13.1 Erlang and C code files are identical. Users have no need to upgrade.

Thanks to Shiro Kawai for the notification of the seed initialization algorithm issue. 

## Travis CI build status for the master branch

[![Build Status](https://travis-ci.org/jj1bdx/sfmt-erlang.svg?branch=master)](https://travis-ci.org/jj1bdx/sfmt-erlang)

## License

Copyright (c) 2010-2017 Kenji Rikitake and Kyoto University. All rights
reserved.

Copyright (c) 2006,2007 Mutsuo Saito, Makoto Matsumoto and Hiroshima
University. All rights reserved.

See the file `LICENSE` for the license (new/simplified BSD license).

## Details of SFMT algorithm

This software is based on SFMT ver. 1.3.3 (SIMD oriented Fast Mersenne
Twister(SFMT)) by Mutsuo Saito (Hiroshima University) and Makoto Matsumoto
(Hiroshima University). See
<http://www.math.sci.hiroshima-u.ac.jp/~m-mat/MT/SFMT/index.html> for the further details.

Note well: only (2^19937 - 1) period is supported.

## Available modules

* module `sfmt`: SFMT with C NIFs
* module `sfmt_pure`: SFMT with pure Erlang
* See [CHANGES.md](https://github.com/jj1bdx/sfmt-erlang/blob/master/CHANGES.md) for the detail of miscellaneous changes

## C NIFs

* Compiler requirement: C99 support (modern Clang and gcc will do): `<inttypes.h>` required
* See `c_src/sfmt_nif.c` for the details
* Also refer to sfmt-extstate at <http://github.com/jj1bdx/sfmt-extstate>
* The version number of this NIF is 101 (see `NIF_LOAD_INFO` macro value)

## Tested platforms

* FreeBSD/amd64 11.1-PRERELEASE with Erlang/OTP 20.0
* macOS 10.12.5 with Erlang/OTP 20.0

## Make options (of erlang.mk)

* `Makefile` works on both BSD/GNU make
* `Makefile.sfmt` is the real GNU make file; edit this file for modification
* Building: `make`
* C NIF Documentation: `make c_doc` (requires Doxygen 1.8.3.1 or later)
* Erlang Documentation: `make docs`
* Testing: `make tests`
* Execution speed benchmark: `make speed`
* See also [erlang.mk](https://github.com/extend/erlang.mk) for the details

## hex.pm support

* NIF building errors fixed (0.12.7 and later)
* Package name: `sfmt`
* See `mix.exs`
* Note: all builds including C and Erlang source compilation are done with `make` with `erlang.mk`

## API compatible with the rand module

    seed/3, uniform/0, uniform/1, uniform_s/1, uniform_s/3 

## On HiPE usage

* sfmt module is NIFnized so does not coexist with HiPE
* On the other hand, `sfmt_pure` module can be compiled with HiPE or `+native "+{hipe, [o3]}"` erlc compile option, which will result in 40% to 100% speedup on 64-bit machines

## TODO

* No more new feature; more load testing needed
* More documentation
* Code upgrading/reloading behavior check

## Code authors:

* Kenji Rikitake
* Mutsuo Saito
* Makoto Matsumoto
* Dan Gudmundsson
* Michael Truog
* Michael Chmielewski
* David Whitlock
* Pierre Fenoll

## THANKS to:

* Dave "dizzyd" Smith
* Tuncer Ayaz
* Tim Bates (random\_mt.erl implementator of Mersenne Twister)
* Richard O'Keefe
* Yurii Rashkovskii
* Kostis Sagonas
* Michael Gebetsroither
* Shunichi Shinohara
* Loïc Hoguin
* @timCF of GitHub
* Shiro Kawai

## ACKNOWLEDGMENTS

During the compatibility test of this software, Kenji Rikitake
used the supercomputer service provided by Academic Center for
Computing and Media Studies (ACCMS), Kyoto University.

Erlang Solutions kindly gave Kenji Rikitake
an opportunity to give a presentation
about this software at Erlang Factory SF Bay 2011.

Pepabo R&D Institute of GMO Pepabo, Inc.
kindly gives the financial support for this project.
