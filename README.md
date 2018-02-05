# VLQ
<!--- All this should be commented out
| **PackageEvaluator**    
|:-----------------------:
| [![][pkg-img]][pkg-url]
-->
| **Build Status**                                                                                  |
|:-------------------------------------------------------------------------------------------------:|
| [![][travis-img]][travis-url] [![][coveralls-img]][coveralls-url] [![][codecov-img]][codecov-url] |

*A Julia implementation of variable-length quantity*

VLQ?
====

A '''variable-length quantity''' ('''VLQ''') is a [universal code](https://en.wikipedia.org/wiki/Universal_code_(data_compression)) that uses an arbitrary number of [binary](https://en.wikipedia.org/wiki/Binary_code) [octet](https://en.wikipedia.org/wiki/Octet_(computing))s (eight-bit bytes) to represent an arbitrarily large integer. It was defined for use in the standard [MIDI file](https://en.wikipedia.org/wiki/MIDI_file) format (MIDI File Format [Variable Quantities](https://web.archive.org/web/20051129113105/http://www.borg.com/~jglatt/tech/midifile/vari.htm)) to save additional space for a resource constrained system, and is also used in the later [Extensible Music Format (XMF)](https://en.wikipedia.org/wiki/Extensible_Music_Format_(XMF)).

It is also used in [ASN.1](https://en.wikipedia.org/wiki/ASN.1) BER encoding to encode tag numbers and [Object Identifiers](http://www.itu.int/ITU-T/studygroups/com17/languages/X.690-0207.pdf). It is also used in the [WAP](https://en.wikipedia.org/wiki/Wireless_Application_Protocol) environment, where it is called '''variable length unsigned integer''' or '''uintvar'''. The [DWARF debugging format](http://www.dwarfstd.org/) defines a variant called [LEB128](https://en.wikipedia.org/wiki/LEB128) (or '''ULEB128''' for unsigned numbers), where the least significant group of 7 bits are encoded in the first byte and the most significant bits are in the last byte (so effectively it is the little-endian analog of a variable-length quantity). [Google Protocol Buffers](https://en.wikipedia.org/wiki/Protocol_Buffers) use a similar format to have compact representation of integer values [ref](http://code.google.com/apis/protocolbuffers/docs/encoding.html), as does [Oracle](https://en.wikipedia.org/wiki/Oracle_Corporation) Portable Object Format ([POF](http://docs.oracle.com/cd/E14526_01/coh.350/e14509/apppifpof.htm)) and the [Microsoft .NET Framework](https://en.wikipedia.org/wiki/.NET_Framework) "7-bit encoded int" in the '''BinaryReader''' and '''BinaryWriter''' classes. [System.IO.BinaryWriter.Write7BitEncodedInt(int)](http://msdn.microsoft.com/en-us/library/system.io.binarywriter.write7bitencodedint.aspx) method and [System.IO.BinaryReader.Read7BitEncodedInt()](http://msdn.microsoft.com/en-us/library/system.io.binaryreader.read7bitencodedint.aspx) method

It is also used extensively in web browsers for source mapping – which contain a lot of integer line & column number mappings – to keep the size of the map to a minimum ("[Introduction to javascript source maps](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)"). Support for encoding/decoding the variable length quantity format described in the spec at [here](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/)

General structure
=================

The encoding assumes an [MSB](https://en.wikipedia.org/wiki/Endianness) (also commonly known as the [sign bit](https://en.wikipedia.org/wiki/Sign_bit])) of octet (often an eight-bit byte), is reserved to indicate whether another VLQ octet follows.

Zigzag encoding
===============

An alternative way to encode negative numbers is to use the least-significant bit for sign. This is notably done for Google Protocol Buffers, and is known as a '''zigzag encoding''' for signed integers [ref](https://developers.google.com/protocol-buffers/docs/encoding#types). Naively encoding a signed integer using [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) means that &minus;1 is represented as an unending sequence of ```...11```; for fixed length (e.g., 64-bit), this corresponds to an integer of maximum length. Instead, one can encode the numbers so that encoded 0 corresponds to 0, 1 to &minus;1, 10 to 1, 11 to &minus;2, 100 to 2, etc.: counting up alternates between nonnegative (starting at 0) and negative (since each step changes the least-significant bit, hence the sign), whence the name "zigzag encoding". Concretely, transform the integer as ```(n << 1) ^ (n >> k - 1)``` for fixed ''k''-bit integers.

## Installation
<!--
The package is registered in `METADATA.jl` and so can be installed with `Pkg.add`.

```julia
julia> Pkg.add("VLQ")
```

or
-->

```julia
julia> Pkg.clone("git://github.com/scientech-com-ua/VLQ.jl.git")
```

## Contributing and Questions

Contributions are very welcome, as are feature requests and suggestions. Please open an
[issue][issues-url] if you encounter any problems or would just like to ask a question.

[travis-img]: https://travis-ci.org/scientech-com-ua/VLQ.jl.svg?branch=master
[travis-url]: https://travis-ci.org/scientech-com-ua/VLQ.jl

[coveralls-img]: https://coveralls.io/repos/scientech-com-ua/VLQ.jl/badge.svg?branch=master&service=github
[coveralls-url]: https://coveralls.io/github/scientech-com-ua/VLQ.jl?branch=master

[codecov-img]: https://codecov.io/gh/scientech-com-ua/VLQ.jl/branch/master/graph/badge.svg
[codecov-url]: https://codecov.io/gh/scientech-com-ua/VLQ.jl

[issues-url]: https://github.com/scientech-com-ua/VLQ.jl/issues

[pkg-0.4-img]: http://pkg.julialang.org/badges/VLQ_0.4.svg
[pkg-0.4-url]: http://pkg.julialang.org/?pkg=VLQ
[pkg-0.5-img]: http://pkg.julialang.org/badges/VLQ_0.5.svg
[pkg-0.5-url]: http://pkg.julialang.org/?pkg=VLQ
[pkg-0.6-img]: http://pkg.julialang.org/badges/VLQ_0.6.svg
[pkg-0.6-url]: http://pkg.julialang.org/?pkg=VLQ
