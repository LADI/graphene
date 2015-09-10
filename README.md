# Graphene
###  A thin layer of types for graphic libraries

[![Build Status](https://travis-ci.org/ebassi/graphene.svg?branch=master)](https://travis-ci.org/ebassi/graphene)

When creating graphic libraries you most likely end up dealing with points
and rectangles. If you're particularly unlucky, you may end up dealing
with affine matrices and 2D transformations. If you're writing a graphic
library with 3D transformations, though, you are going to hit the jackpot:
4x4 matrices, projections, transformations, vectors, and quaternions.

Most of this stuff exists, in various forms, in other libraries, but it
has the major drawback of coming along with the rest of those libraries,
which may or may not be what you want. Those libraries are also available
in various languages, as long as those languages are C++; again, it may or
may not be something you want.

For this reason, I decided to write the thinnest, smallest possible layer
needed to write a canvas library; given its relative size, and the
propensity for graphics libraries to have a pun in their name, I decided
to call it Graphene.

This library provides types and their relative API; it does not deal with
windowing system surfaces, drawing, scene graphs, or input. You're
supposed to do that yourself, in your own canvas implementation, which is
the whole point of writing the library in the first place.

### Dependencies

Graphene has little dependencies.

Graphene contains optimizations for speeding up vector operations; those
optimizations are optional, and used only if both Graphene was compiled
with support for them *and* if the system you're running on has them.
Currently, Graphene supports the following platform-specific fast paths:

 * Streaming SIMD Extensions (SSE) 2
 * ARM NEON
 * GCC vector extensions

In the remote case in which none of these optimizations are available,
Graphene will fall back to a naive scalar implementation.

Graphene can, optionally, provide types for integrating with GObject
properties and signals, as well as introspection information for its
use with other languages through introspection-based bindings.

### Installation

In order to build and install Graphene you will need development tools and
the headers of the dependencies.

First of all, clone the Git repository:

    $ git clone git://github.com/ebassi/graphene
    $ cd graphene

Then run:

    $ ./autogen.sh
    $ make
    $ make check
    # make install

It is possible, when building Graphene, to disable specific optimizations by
passing options to the `configure` script:

 * `--disable-sse2` - will disable the SSE2 fast paths
 * `--disable-arm-neon` - will disable the ARM NEON fast paths
 * `--disable-gcc-vector` - will disable the GCC vector intrinsics

If you don't plan on generating introspection data, use `--disable-introspection`
when configuring Graphene; similarly, if you don't plan on using GObject with
Graphene, use `--disable-gobject-types`. Disabling GObject types will also
automatically disable generating introspection data.

You can also disable building the test suite and the benchmark suite, using
the `--disable-tests` and `--disable-benchmarks` configuration switches
respectively.

See the output of `configure --help` for more information on the available
configuration switches and environment variables.

#### Installing on Windows

In order to build on Windows, it's recommended to use the
[MSYS2](http://sourceforge.net/projects/msys2/) environment.

First,  install MSYS2

Then, use `pacman` to set up the build environment, by 
installing the necessary prerequisites:

    $ pacman -Sy
    $ pacman -S make
    $ pacman -S automake
    $ pacman -S autoconf
    $ pacman -S libtool
    $ pacman -S pkg-config
    $ pacman -S gtk-doc		# optional
    $ pacman -S glib2		# optional

Then, clone the Graphene repository and build as usual by following the
instructions in the section above.

### Contributing

If you found a bug (which I'm sure there will be plenty), or if you want
to add your own pet feature, then follow these steps:

 1. Fork the [ebassi/graphene](https://github.com/ebassi/graphene) repo
 2. Fix bugs or add new features and push them to your clone
 3. Open [a new issue](https://github.com/ebassi/graphene/issues/new)
 4. Open [a pull request](https://github.com/ebassi/graphene/pulls)
 5. Wait for me to give feedback on the pull request
 6. Celebrate when your code gets merged

That's pretty much it.

Please, respect the coding style when writing patches for Graphene. The
coding style can be immediately gleaned from the existing code, but here is
a short version:

 * Indentation is made of spaces, and only spaces — **no tabs**
 * Each indentation level is 2 spaces
 * Curly braces for blocks go on a separate indentation level
  * Except for functions
 * Do not use curly braces for single-statement blocks
 * When declaring or defining a function, arguments go on separate
   lines
 * When calling a function never break a line between the function
   name and the opening parenthesis, and between the opening parenthesis
   and the first argument
 * Leave a space between functions and parenthesis

A more comprehensive coding style document is available in the `docs`
directory.

## Documentation

### Release notes

The release notes are available on the Graphene
[wiki](https://github.com/ebassi/graphene/wiki/Release-Notes).

### Available types

Graphene provides common types needed to handle 3D transformations:

 * [2D points](http://ebassi.github.io/graphene/docs/graphene-Point.html)
 * [3D points](http://ebassi.github.io/graphene/docs/graphene-Point3D.html)
 * [triangles](http://ebassi.github.io/graphene/docs/graphene-Triangle.html)
 * [rectangles](http://ebassi.github.io/graphene/docs/graphene-Rectangle.html)
 * [quads](http://ebassi.github.io/graphene/docs/graphene-Quad.html)
 * [quaternions](http://ebassi.github.io/graphene/docs/graphene-Quaternion.html)
 * [euler angles](http://ebassi.github.io/graphene/docs/graphene-Euler.html)
 * [vectors](http://ebassi.github.io/graphene/docs/graphene-Vectors.html) (2, 3, or 4-sized)
 * [matrices](http://ebassi.github.io/graphene/docs/graphene-Matrix.html)
 * [planes](http://ebassi.github.io/graphene/docs/graphene-Plane.html)
 * [axis aligned bounding boxes](http://ebassi.github.io/graphene/docs/graphene-Box.html)
 * [spheres](http://ebassi.github.io/graphene/docs/graphene-Sphere.html)
 * [frustums](http://ebassi.github.io/graphene/docs/graphene-Frustum.html)

Graphene also provides its low-level SIMD [vector](http://ebassi.github.io/graphene/docs/graphene-SIMD-vector.html)
and [matrix](http://ebassi.github.io/graphene/docs/graphene-SIMD-matrix.html)
types, which are used to implement the API above.

All types can be placed on the stack, but provide allocation/free functions
for working on the heap as well. The contents of all structure types, unless
noted otherwise, should be considered private, and should never be accessed
directly.

The full API reference for Graphene is [available online](http://ebassi.github.io/graphene/docs/).

### License

Graphene is released under the terms of the MIT/X11 license.

See the `LICENSE` file for more details.
