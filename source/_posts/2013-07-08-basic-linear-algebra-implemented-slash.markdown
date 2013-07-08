---
layout: post
title: "GSoC FLINT project accepted and the start of (heavily delayed, sorry!) blog posts"
date: 2013-07-08 00:02
comments: true
categories: [gsoc, flint]
---

Hello! As the end of semester took much more time than I imagined, I ended up delaying writing posts about my progress on GSoC, though this will be hopefully corrected by a lot of updates in the following days discussing both the state of the work and the design decisions I took/shall have to take during the development of the library during this summer/winter.

All forms of feedback and comments are both welcomed and encouraged!

Status
------

The code is currently available on [the `d_mat` branch of my flint2 fork on Github](https://github.com/andrioni/flint2/tree/d_mat). Most of the basic features for double-precision vectors and matrices is implemented already, though not yet optimized. There are also some tests written, but I'm not yet satisfied with their broadness, and this will probably lead to the implementation of more functions to generate random doubles. The existing code for MPFR-backed vectors was also extended, but most tests for it aren't written yet.

Documentation is currently mostly sketched and uncommitted, as I'm awaiting to stabilize a bit the planned API before finishing it.

List of issues faced so far
---------------------------

- The function currently being used to generate doubles, `d_randtest`, generates numbers only between [0.5, 1), though Fredrik has already given orientation on how to generate other random doubles from that;
- No special care has been taken to deal with NaNs and infinities, though according to Bill this isn't really important to most applications in number theory;
- The old `mpfr_vec` header was a bit confusing, as some functions were using `__mpfr_struct` instead of `mpfr` (which is just typedef'd to `__mpfr_struct` on `flint.h`, and is much more readable). I decided to standardize them on `mpfr` for clarity, and added the `const` keyword where it was possible;
- There is already `_mpfr_vec_scalar_product` function written, while I decided to name the double precision version `_d_vec_innerprod`, and it would be better to have one standard name. Both of them seem reasonable to me, and I strongly welcome any input on which one is better!
- It seems that the old code was written based on MPFR 2.4 (or older versions), and it seems to be the last version supported by MinGW (though MinGW-w64 supports 3.1). I'm currently trying to keep compatibility as much as I can, but it could be good to formally define which MPFR version flint requires;
- A related issue is the use of `GMP_RNDN` as the rounding mode constant used instead of `MPFR_RNDN` (available on 3.0 and above).

List of possible extra ideas for the following weeks
----------------------------------------------------

- Using either/both the [GCC Compile Farm](http://gcc.gnu.org/wiki/CompileFarm) or [Travis CI](https://travis-ci.org/) to automatically run tests on different platforms and compilers;
- Writing something to track performance of the linear algebra functions to help benchmarking and to find more easily performance regressions.

To work!
