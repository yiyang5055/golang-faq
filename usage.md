### Google内部使用Go吗？

是的。在Google内部Go在生产环境使用非常广泛。一个最简单的例子就是[golang.org](https://golang.org/),它的后端就是用Go写的，它是运行在Google App Engine生产环境中的一个[`godoc`](https://golang.org/cmd/godoc) 相关服务。

一个更重要的例子是Google的下载服务`dl.google.com`，它提供了Chrome二进制文件以及其他大体积安装包的分发服务。

Go不仅仅是在Google内部作为一门编程语言，绝非这样，它包括在[site reliability engineering (SRE)](https://talks.golang.org/2013/go-sreops.slide) 和大规模可扩展数据处理系统等其他很多领域内扮演了很重要的角色。

### 有其他公司使用Go吗？

Go的使用在全球范围内都在增长，尤其是在云计算领域，但绝不仅仅于此，比如Docker和Kubernetes两个云计算基础设施项目都是用Go写的，而且还有更多其他的项目。

然而，不仅仅是在云计算。Go的[Wiki](https://github.com/golang/go/wiki/GoUsers)有一个经常更新的页面,这里面就列出了一些使用Go的公司。还有一个[页面](https://github.com/golang/go/wiki/SuccessStories),上面有很多成功使用Go的公司和项目的故事。

### Go如何链接C/C++代码？

同时使用C和Go是有可能的，但是这并不是一个正常使用方式，而且需要一些特定的接口。Go中使用C也就意味着放弃了Go提供的内存安全和栈管理这些特性。有时候必须使用C库去解决一个问题，但是这样做比纯Go代码有更高的风险，必须小心应对。

如果你需要在Go中需要使用C语言，怎么使用取决你使用哪种编译器。Go团队实现了三种编译器，`gc`-默认编译器，`gcc-go`-使用GCC做后端，还有一种不太稳定的`gollvm`-使用LLVM基础设施构建的编译器。

`gc`使用了一种不同于C的调用约定和链接， 所以不能直接使用C代码。[`cgo`](https://golang.org/cmd/cgo/) 提供了一种叫做”外部函数接口“的机制允许在Go中安全的调用C库。SWIG拓展了C++库以支持外部调用

你还可以使用cgo和SWIG与Gccgo和gollvm。由于它们使用传统的API，所以也可以非常小心地将代码直接和使用GCC/ llvm编译过的C或c++程序直接链接。但是，安全地执行此操作需要理解所有相关语言的调用约定，并且也要关注在从Go调用C或c++时对堆栈的限制。

### 有哪些支持Go的IDE？

Go项目不包括IDE，但是语言和标准库的源码非常容易分析，因此，很多著名的编辑器和IDE直接或者通过插件的形式支持Go。

Vim, VSCode, Atom, Eclipse, Sublime, IntelliJ等这些著名的编辑器和IDE都对Go做了很好的支持。你可以选择你喜欢的一个环境高效的编码。

### Go支持Google的protocol buffers吗?

有一个独立的开源项目, 提供了必要的编译组件和库。地址：https://github.com/golang/protobuf/

### 我可以将Go的站点翻译成其他语言吗？

当然。我们鼓励开发者用他们的语言去构建Go语言相关网站。 然而如果选择添加google logo或者brand到你的网站(在 [golang.org](https://golang.org/)上没有出现的), 那么就需要遵守 [www.google.com/permissions/guidelines.html](https://studygolang.com/articles/www.google.com/permissions/guidelines.html)上声明的准则。
