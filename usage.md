### Is Google using Go internally?

Yes. Go is used widely in production inside Google. One easy example is the server behind [golang.org](https://golang.org/). It's just the [`godoc`](https://golang.org/cmd/godoc) document server running in a production configuration on [Google App Engine](https://developers.google.com/appengine/).

A more significant instance is Google's download server, `dl.google.com`, which delivers Chrome binaries and other large installables such as `apt-get` packages.

Go is not the only language used at Google, far from it, but it is a key language for a number of areas including[site reliability engineering (SRE)](https://talks.golang.org/2013/go-sreops.slide) and large-scale data processing.

### What other companies use Go?

Go usage is growing worldwide, especially but by no means exclusively in the cloud computing space. A couple of major cloud infrastructure projects written in Go are Docker and Kubernetes, but there are many more.

It's not just cloud, though. The Go Wiki includes a [page](https://github.com/golang/go/wiki/GoUsers), updated regularly, that lists some of the many companies using Go.

The Wiki also has a page with links to [success stories](https://github.com/golang/go/wiki/SuccessStories) about companies and projects that are using the language.

### Do Go programs link with C/C++ programs?

It is possible to use C and Go together in the same address space, but it is not a natural fit and can require special interface software. Also, linking C with Go code gives up the memory safety and stack management properties that Go provides. Sometimes it's absolutely necessary to use C libraries to solve a problem, but doing so always introduces an element of risk not present with pure Go code, so do so with care.

If you do need to use C with Go, how to proceed depends on the Go compiler implementation. There are three Go compiler implementations supported by the Go team. These are `gc`, the default compiler, `gccgo`, which uses the GCC back end, and a somewhat less mature `gollvm`, which uses the LLVM infrastructure.

`Gc` uses a different calling convention and linker from C and therefore cannot be called directly from C programs, or vice versa. The [`cgo`](https://golang.org/cmd/cgo/) program provides the mechanism for a “foreign function interface” to allow safe calling of C libraries from Go code. SWIG extends this capability to C++ libraries.

You can also use `cgo` and SWIG with `Gccgo` and `gollvm`. Since they use a traditional API, it's also possible, with great care, to link code from these compilers directly with GCC/LLVM-compiled C or C++ programs. However, doing so safely requires an understanding of the calling conventions for all languages concerned, as well as concern for stack limits when calling C or C++ from Go.

### What IDEs does Go support?

The Go project does not include a custom IDE, but the language and libraries have been designed to make it easy to analyze source code. As a consequence, most well-known editors and IDEs support Go well, either directly or through a plugin.

The list of well-known IDEs and editors that have good Go support available includes Emacs, Vim, VSCode, Atom, Eclipse, Sublime, IntelliJ (through a custom variant called Goland), and many more. Chances are your favorite environment is a productive one for programming in Go.

### Does Go support Google's protocol buffers?

A separate open source project provides the necessary compiler plugin and library. It is available at[github.com/golang/protobuf/](https://github.com/golang/protobuf).

### Can I translate the Go home page into another language?

Absolutely. We encourage developers to make Go Language sites in their own languages. However, if you choose to add the Google logo or branding to your site (it does not appear on [golang.org](https://golang.org/)), you will need to abide by the guidelines at [www.google.com/permissions/guidelines.html](https://www.google.com/permissions/guidelines.html)