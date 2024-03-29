#### 项目的目的是什么? 

从Go语言诞生到现在虽然只有十来年，但是编程世界和当初早已不相同。在十几年前，生产软件通常是用C++或者Java语言编写，那时候Github还不存在，大多数计算机还没有多处理器，除了Visual Studio和Eclipse几乎没有其他可用的ide或高级工具，更不用说在互联网上免费使用的。

与此同时，我们开发服务器软件所用的编程语言复杂度越来越高。自从C、C++和Java语言被首次开发出来，计算机的运行速度已经有了非常大的提高，但是实际上编程语言却没有变的更加高级。此外，很明显的是多处理器正在变的越来越普遍，但是大多数编程语言在效率和安全方面并没有提供进一步的帮助。

我们决定退一步来思考问题，随着技术的发展，软件工程在未来将会面临哪些主要问题，以及一种新的语言将如何来帮助解决这些问题。例如，多核处理器的兴起表明，新的编程语言应该为某些并发或者并行提供一流的支持。为了在大型并发程序中更加容易的管理资源，编程语言需要垃圾收集或者至少某种安全的自动内存管理。

这些考虑导致了一系列的[讨论](https://commandcenter.blogspot.com/2017/09/go-ten-years-and-climbing.html)，Go首先作为一些列想法和愿望出现，然后才是作为语言出现。一个首要的的目标就是希望Go通过启用工具、自动执行常见任务(例如代码格式化)在大型代码库上消除一些障碍，从而为编人员提供更多的帮助。

这里有一篇文章，对Go的目标以及如何来实现这些目标进行了更加广泛的描述: [Go at Google: Language Design in the Service of Software Engineering](https://talks.golang.org/2012/splash.article).

#### 项目的历史是什么？

Robert Griesemer, Rob Pike和Ken Thompson于2007年9月21日开始在白板上勾画新编程语言的目标。在几天之内，这些目标就落实形成了一个去实现这些目标的计划以及它将会是什么的一个公平的想法。设计工作在进行的同时兼职一些其他不相关的工作。到2008年1月，Ken Thompson已经开始着手设计一个输出结果为C语言的编译器，用以探索思路。到了年中，这个项目已经成为了全职项目，并且有充足准备来开发其编译器。在2008年5月，Ian Taylor独立开发了一个符合Go草案的GCC的前端。后半年加入的Russ Cox，通过原型开始实现Go的语言规则及一些基础库。

在2009年11月10日，Go成为了一个开源项目。来自开源社区无数的人贡献着他们的想法、讨论和代码。现在已经有着来自全球数百万的Go语言使用者，并且人数每天都在增长着。Go的成功远远超过了我们的预期。

#### gopher吉祥物的起源是什么？

吉祥物和logo是由[Renée French](https://reneefrench.blogspot.com/)设计，他也是[Glenda](https://9p.io/plan9/glenda.html), the Plan 9 bunny的设计者。有一篇关于gopher的[博客文章](https://blog.golang.org/gopher)解释了她是如何从几年前她使用过的[WFMU](https://wfmu.org/) t恤设计中获得的有关吉祥物和logo的设计灵感。标识和吉祥物遵守 [Creative Commons Attribution 3.0](https://creativecommons.org/licenses/by/3.0/)许可协议。

Gopher有一个[人物设计](https://golang.org/doc/gopher/modelsheet.jpg)表来说明它的特点和如何正确的表示它们。2016年，Renee在Gophercon的一次演讲中首次展示了改人物设计。它是一个独特的Go gopher，而不是普通的老gopher.

#### 这个编程语言叫Go还是Golang?

这个编程语言叫做Go. "golang"这个名字的出现是因为网站是[golang.org](https://golang.org/),而不是go.org，go.org我们是无法访问的。更多的时候大家把golang这个名字作为一个方便使用的标签。例如，在Twitter上这个语言的的标签是#golang。不管怎么样，这个编程语言的名字就叫做Go.

一个边注：尽管[官方logo](https://blog.golang.org/go-brand)是两个大写字母，但是这个编程语言的名字就叫做Go,而不是GO.

#### 为什么要创造一门新的编程语言？

Go之所以诞生，是对Google现有编程语言和编程环境不满的一种驱动使然。编程变的越来越困难，并且在语言的选择上变的迷茫。编译效率、运行效率和语言的易用性在同一个主流的语言上是不可同时兼得的。开发者倾向选择诸如Python和 JavaScript这类易于开发的动态类型语言，而不是C++、Java这类更加安全和有执行效率的语言。

并非只有我们有这样的担忧。多年来，编程语言领域一片宁静，而仅Go是Rust, Elixir, Swift等新语言中的一个，他们使编程语言开发在主流领域里再次活跃起来。

Go尝试将解释型、动态型语言的易用性和静态、编译型语言的效率、安全结合起来。它的目标是将Go打造成支持网络以及多核编程的现代化编程语言。最后，希望Go是快速的：它应该在单台机器上最多需要几秒钟的时间完成一个大型工程的编译工作。要达成这样的目标还需要解决大量这样的问题：一个有表达力并且轻量的类型系统；并发和垃圾收集；严格的依赖规范等等。现有的库或者工具并不能很好的解决这些问题，因此一个新的语言应运而生。

[Go at Google](https://talks.golang.org/2012/splash.article) 这篇文章讨论了Go语言诞生的背景以及动机，回答了更多在本篇FAQ中问题的细节。

#### Go的原型是什么？

Go很大程度上属于C家族(基础语法)，参考并吸收了Pascal/Modula/Oberon这些语言的优点(declarations, packages)，再结合来自其他像Newsqueak和Limbo语言Tony Hoare的CSP理论。然而它是一门全新的编程语言。每一个被认可的语言在设计的时候都要考虑到开发者可以做什么以及怎么做，至少我们创造的语言应该更加有效率，这也意味着更加的有乐趣。

#### 设计的指导原则是什么？

Go在设计的时候，在Google内部，Java和C++语言是服务端领域使用最多的语言。我们感觉到这些语言包含了太多的记账和重复性的工作。一些开发者倾向使用像Python这类牺牲效率和类型安全的动态语言。我们认为新的语言应该尽可能做到高效、类型安全和流畅。

Go尝试减少输入的代码数量。透过它的设计, 我们尝试减少混乱和复杂度。在Go中没有前置声明和头文件；所有对象、变量都只声明一次。初始化是简洁、自动而且易用的。语法干净, 关键字轻量。使用 `:=`声明并定义的方式来简化类似`foo.Foo* myFoo = new(foo.Foo)`这样的表达方式。而最基本的，Go的类型系统是没有层级结构的，这样也就不用去声明他们之间的关系。这种简化机制使得Go变的富有表达力和容易理解。

另外一个重要的原则就是概念正交。一个方法可以被任何类型来实现；结构体表示数据而接口用来表示抽象等等。正交性使得操作组合时更容易理解其中发生了什么。
