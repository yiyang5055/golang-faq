#### 项目的目的是什么? 

从Go语言诞生到现在虽然只有十来年，但是编程时间和当初早已不相同。在十几年前，生产软件通常是用C++或者Java语言编写，那时候Github还不存在，大多数计算机还没有多处理器，除了Visual Studio和Eclipse几乎没有其他可用的ide或高级工具，更不用说在互联网上免费试用的。

与此同时，我们开发服务器软件所用的编程语言复杂度越来越高。自从C、C++和Java语言被首次开发出来，计算机的运行速度已经有了非常大的提高，但是实际上编程语言却没有变的更加高级。此外，很明显的是多处理器正在变的越来越普遍，但是大多数编程语言在效率和安全方面并没有提供进一步的帮助。

我们决定退一步来思考问题，随着技术的发展，软件工程在未来将会面临哪些主要问题，以及一种新的语言将如何人来帮助解决这些问题。例如，多核处理器的兴起表明，新的编程语言应该为某些并发或者并行提供一流的支持。为了在大型并发程序中更加容易的管理资源，编程语言需要垃圾收集或者至少某种安全的自动内存管理。

These considerations led to [a series of discussions](https://commandcenter.blogspot.com/2017/09/go-ten-years-and-climbing.html) from which Go arose, first as a set of ideas and desiderata, then as a language. An overarching goal was that Go do more to help the working programmer by enabling tooling, automating mundane tasks such as code formatting, and removing obstacles to working on large code bases.

这些考虑导致了一系列的[讨论](https://commandcenter.blogspot.com/2017/09/go-ten-years-and-climbing.html)，Go首先作为一些列想法和愿望出现，然后才是作为语言出现。一个首要的的目标就是希望Go通过启用工具、自动执行常见任务(例如代码格式化)在大型代码库上消除一些障碍，从而为正在工作的编程人员提供更多的帮助。

这里有一篇文章，对GO的目标以及如何来实现这些目标进行了更加广泛的描述： [Go at Google: Language Design in the Service of Software Engineering](https://talks.golang.org/2012/splash.article).

#### What is the history of the project? 

Robert Griesemer, Rob Pike and Ken Thompson started sketching the goals for a new language on the white board on September 21, 2007. Within a few days the goals had settled into a plan to do something and a fair idea of what it would be. Design continued part-time in parallel with unrelated work. By January 2008, Ken had started work on a compiler with which to explore ideas; it generated C code as its output. By mid-year the language had become a full-time project and had settled enough to attempt a production compiler. In May 2008, Ian Taylor independently started on a GCC front end for Go using the draft specification. Russ Cox joined in late 2008 and helped move the language and libraries from prototype to reality.

Go became a public open source project on November 10, 2009. Countless people from the community have contributed ideas, discussions, and code.

There are now millions of Go programmers—gophers—around the world, and there are more every day. Go's success has far exceeded our expectations.

#### 这是项目的历史是什么？

Robert Griesemer, Rob Pike和Ken Thompson从2007年9月21日开始在白板上为新的编程语言勾画目标。

#### gopher吉祥物的起源是什么？

吉祥物和logo是由[Renée French](https://reneefrench.blogspot.com/)设计，他也是[Glenda](https://9p.io/plan9/glenda.html), the Plan 9 bunny的设计者。有一篇关于gopher的[博客文章](https://blog.golang.org/gopher)解释了她是如何从几年前她使用过的[WFMU](https://wfmu.org/) t恤设计中获得的有关吉祥物和logo的设计灵感。标识和吉祥物遵守 [Creative Commons Attribution 3.0](https://creativecommons.org/licenses/by/3.0/)许可协议。

Gopher有一个[人物设计](https://golang.org/doc/gopher/modelsheet.jpg)表来说明它的特点和如何正确的表示它们。2016年，Renee在Gophercon的一次演讲中首次展示了改人物设计。它是一个独特的Go gopher，而不是普通的老gopher.

#### 这个编程语言叫Go还是Golang?

这个编程语言叫做Go. "golang"这个名字的出现是因为网站是[golang.org](https://golang.org/),而不是go.org，go.org我们是无法访问的。更多的时候大家把golang这个名字作为一个方便使用的标签。例如，在Twitter上这个语言的的标签是#golang。不管怎么样，这个编程语言的名字就叫做Go.

一个边注：尽管[官方logo](https://blog.golang.org/go-brand)是两个大写字母，但是这个编程语言的名字就叫做Go,而不是GO.

#### Why did you create a new language?

Go was born out of frustration with existing languages and environments for the work we were doing at Google. Programming had become too difficult and the choice of languages was partly to blame. One had to choose either efficient compilation, efficient execution, or ease of programming; all three were not available in the same mainstream language. Programmers who could were choosing ease over safety and efficiency by moving to dynamically typed languages such as Python and JavaScript rather than C++ or, to a lesser extent, Java.

We were not alone in our concerns. After many years with a pretty quiet landscape for programming languages, Go was among the first of several new languages—Rust, Elixir, Swift, and more—that have made programming language development an active, almost mainstream field again.

Go addressed these issues by attempting to combine the ease of programming of an interpreted, dynamically typed language with the efficiency and safety of a statically typed, compiled language. It also aimed to be modern, with support for networked and multicore computing. Finally, working with Go is intended to be *fast*: it should take at most a few seconds to build a large executable on a single computer. To meet these goals required addressing a number of linguistic issues: an expressive but lightweight type system; concurrency and garbage collection; rigid dependency specification; and so on. These cannot be addressed well by libraries or tools; a new language was called for.

The article [Go at Google](https://talks.golang.org/2012/splash.article) discusses the background and motivation behind the design of the Go language, as well as providing more detail about many of the answers presented in this FAQ.

#### What are Go's ancestors?

Go is mostly in the C family (basic syntax), with significant input from the Pascal/Modula/Oberon family (declarations, packages), plus some ideas from languages inspired by Tony Hoare's CSP, such as Newsqueak and Limbo (concurrency). However, it is a new language across the board. In every respect the language was designed by thinking about what programmers do and how to make programming, at least the kind of programming we do, more effective, which means more fun.

### What are the guiding principles in the design?

When Go was designed, Java and C++ were the most commonly used languages for writing servers, at least at Google. We felt that these languages required too much bookkeeping and repetition. Some programmers reacted by moving towards more dynamic, fluid languages like Python, at the cost of efficiency and type safety. We felt it should be possible to have the efficiency, the safety, and the fluidity in a single language.

Go attempts to reduce the amount of typing in both senses of the word. Throughout its design, we have tried to reduce clutter and complexity. There are no forward declarations and no header files; everything is declared exactly once. Initialization is expressive, automatic, and easy to use. Syntax is clean and light on keywords. Stuttering (`foo.Foo* myFoo = new(foo.Foo)`) is reduced by simple type derivation using the `:=` declare-and-initialize construct. And perhaps most radically, there is no type hierarchy: types just *are*, they don't have to announce their relationships. These simplifications allow Go to be expressive yet comprehensible without sacrificing, well, sophistication.

Another important principle is to keep the concepts orthogonal. Methods can be implemented for any type; structures represent data while interfaces represent abstraction; and so on. Orthogonality makes it easier to understand what happens when things combine.
