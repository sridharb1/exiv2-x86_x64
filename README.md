# exiv2-x86_x64
Visual Studio project files to compile exiv2 on Windows (x86/x64/Release/Debug)

# Motivation #
I use Visual Studio on Windows for my personal product development. I
needed to use exiv2 for a project to sync my photographs in
[Picasa](https://picasa.google.com/)[^1] to
[Piwigo](https://github.com/Piwigo/). I use Piwigo, running on my own
server, because I do not want to share my family photographs in
platforms like facebook, google photos, instagram, whatsapp etc. For
the same privacy concerns, this piwigo gallery on my server is not
public and is accessed through credentials shared only with family.

## Background ##
I started this project quite a while ago (mid-2016). I got it to a
working state by the end of 2019, mostly because it has been hard to
find solid time to devote to this. Needless to say, there was nothing
that did exactly what I wanted and few other projects that did some of
what I needed. I borrowed liberally to cobble up this solution.

One of the main building blocks was exiv2. When I started this
project, support for building on Windows using Visual Studio was quite
good and in particular, [Robin Mills](https://github.com/clanmills),
the then maintainer, was excellent in helping resolve questions.

However, as time progressed, the maintainers changed and their primary
focus turned to Linux and Windows compilation was an afterthought
accomplished by the swiss knife CMake. In reality, CMake, produces
horrible Visual Studio solution/projects that just creates wrappers on
cmake commands, tries to enforce the Linux custom of all dependencies
installed in a single location like /usr/local as well as not
retaining the flexibility of targeting separate x86/x64/Release/Debug
builds while using the same solution/projects.

The native solution/project files were removed from the exiv2
repository and not even added to a contrib folder for others to use, a
restriction that I found particularly galling. The fact that the main
audience didn't squeal much at these steps is not the answer as they
may not care about compiling on Windows using VS. This was a
retrograde step, but they believed that they were going forward.

One can never predict what the world wants. If one is not open to
possibilities and instead, claims that my way is the highway, the
utility of the product is diminished. This project is to help people
who would like to build exiv2 using Visual Studio on Windows, to do
so.

**Note** As the code in the master exiv2 repository changes, these
project files may not work out of the box. However, with small
changes, they could be made to work with the latest.

## Building Philosophy ##
Unlike the linux-style philosophy of installing everything in and then,
linking from, /usr/local-type folders, typically source trees on
Windows are not organized that way. At least, I don't.

I prefer to git clone from the target repository onto my workspace. I
also prefer to get the latest git versions of exiv2's dependencies as
well. This is an important distinction. The usual philosophy would
install released packages (say into C:\Program Files\...) and the
compiled package to link against them.

The advantage with my approach is that I have a transparent solution
with all the dependencies visible explicitly instead of needing to be
added in areas like Additional Libraries. The debugger is able to step
into any area of the code, including the dependencies easily instead
of worrying whether the dependency is release/debug compiled and is
consistent with all the choices that I make for the application (like
static/runtime CRT or x86/x64 or Release/Debug).

This involves creating one solution and including in it, multiple
projects (its own as well as those of the dependencies) and linking
the dependencies through references instead of explicit identification
through path/folder names.

For e.g. exiv2 will be git-cloned into E:\Projects\exiv2. libexpat (a
dependency that is required for compiling exiv2) will be git-cloned to
E:\Projects\libexpat. The exiv2 solution will have the exiv2 project
(it has many more, but mentioning just one for simplifying the
exposition) and the expat-static project (from libexpat). The
expat-static project is added as a reference to the exiv2
project. This make Visual Studio generate the correct dependency
heirarchy and compile correctly.

# Sources #
  * [Exiv2, tested w/ v0.27.2](https://github.com/Exiv2/exiv2)
  * Use my [exiv2-x86_x64](https://github.com/sridharb1/exiv2-x86_x64) to
compile on Windows
  * Place the contents of exiv2-x86_x64 in a folder called build in the exiv2 repository and build using the provided solution. Please note that the dependencies listed below are needed.

# Dependencies #
  * [zlib, tested w/ v1.2.11](https://github.com/madler/zlib.git)
  * [Strawberry Perl, used v5.30.1.1 x64](strawberryperl.com)[^2]
  * [NASM, used 2.14.02 x64](https://www.nasm.us/)[^3]
  * [libexpat, tested w/ v2.2.9](https://github.com/libexpat/libexpat)[^4]
  * [OpenSSL, tested w/ v1.1.1e-DEV](https://github.com/openssl/openssl)[^5]
  * [libssh2, tested w/ v1.9.0](https://github.com/libssh2/libssh2.git)[^6]
  * [curl, tested w/ v7.69.1](https://github.com/curl/curl.git)[^7]
  * [googletest, tested w/ v1.10.x](https://github.com/google/googletest.git)[^8]
  * [libintl (aka gettext), tested with v0.20.1](https://git.savannah.gnu.org/git/gettext.git)[^9]
  * [libiconv, tested w/ v1.16](https://github.com/sridharb1/libiconv-x86_x64)
  * [libssh, tested w/ v0.9.3](https://git.libssh.org/projects/libssh.git/)[^10]
  * [Visual Studio, used Community 2019 edition](https://visualstudio.microsoft.com/)

[^1]: It has since been killed by google. (You can find the last released version here: [Picasa 3.9.141, build 259](https://1drv.ms/u/s!AuE8ZYzPkfMfiEzdlKtVVjN-MReT))

[^2]: I installed the binary distribution. perl is only used by the
openssl makefile to compile. There is no chance of needing to step
into perl code while debugging exiv2.

[^3]: Binary distribution. See [^3].

[^4]: Use my [libexpat-x86_x64](https://github.com/sridharb1/libexpat-x86_x64) to
compile on Windows using Visual Studio

[^5]: Use my [openssl-x86_x64](https://github.com/sridharb1/openssl-x86_x64) to compile on Windows using Visual Studio

[^6]: Use my [libssh2-x86_x64](https://github.com/sridharb1/libssh2-x86_x64.git) to compile on Windows using Visual Studio

[^7]: Use my [curl-x86_x64](https://github.com/sridharb1/curl-x86_x64.git) to
compile on Windows using Visual Studio

[^8]: Use my [googletest-x86_x64](https://github.com/sridharb1/googletest-x86_x64.git) to compile on Windows using Visual Studio

[^9]: Use my [gettext-x86_x64](https://github.com/sridharb1/gettext-x86_x64) to compile on Windows using Visual Studio

[^10]: Use my [libssh-x86_x64](https://github.com/sridharb1/libssh-x86_x64.git) to compile on Windows using Visual Studio
