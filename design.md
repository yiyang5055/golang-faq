### Go有运行时吗？

Go有一个叫做*runtime*的扩展库，它是每个Go程序的一部分。runtime库实现了垃圾收集，并发性，栈管理，和Go语言其他关键特性。runtime对Go语言来说非常重要，类似于C库中的`libc`。

但是重要的是要理解Go的运行时并没有提供一个类似Java那样的虚拟机。而Go程序是被提前编译成本地机器码。因此尽管这个术语经常用来描述程序运行的虚拟环境，但在Go中，”运行时“这个词只是一个提供关键语言服务的名称。

### Unicode标识符是怎么回事?

When designing Go, we wanted to make sure that it was not overly ASCII-centric, which meant extending the space of identifiers from the confines of 7-bit ASCII. Go's rule—identifier characters must be letters or digits as defined by Unicode—is simple to understand and to implement but has restrictions. Combining characters are excluded by design, for instance, and that excludes some languages such as Devanagari.

This rule has one other unfortunate consequence. Since an exported identifier must begin with an upper-case letter, identifiers created from characters in some languages can, by definition, not be exported. For now the only solution is to use something like `X日本語`, which is clearly unsatisfactory.

Since the earliest version of the language, there has been considerable thought into how best to expand the identifier space to accommodate programmers using other native languages. Exactly what to do remains an active topic of discussion, and a future version of the language may be more liberal in its definition of an identifier. For instance, it might adopt some of the ideas from the Unicode organization's [recommendations](http://unicode.org/reports/tr31/) for identifiers. Whatever happens, it must be done compatibly while preserving (or perhaps expanding) the way letter case determines visibility of identifiers, which remains one of our favorite features of Go.

For the time being, we have a simple rule that can be expanded later without breaking programs, one that avoids bugs that would surely arise from a rule that admits ambiguous identifiers.

### 为什么Go没有feature X?

每个语言都包含了一些新颖的特性并且忽略了一些人喜欢的特性。Go被设计成便于阅读，快速编译，概念正交，而且还需要支持并发性和垃圾收集这样的特性。你喜欢的特性被遗漏因为它不合适，因为它影响了编译速度或者设计的清晰，或者因为它使得基础系统模型变的复杂。

如果遗漏的某些特性给你带来了不便，请原谅我们并且研究Go已经支持的特性，你可能会发现补偿你确实某些特性的乐趣。

### 为什么Go没有泛型？

泛型很可能在某个时候被添加。虽然我们知道有些程序员会这样做，但我们并不觉得它们很紧迫。

Go语言现在越来越成熟，某些形式的泛型编程也在考虑中。但是仍然有一些需要注意的地方。参见这篇[文章](https://talks.golang.org/2012/splash.article)能够得到更多的背景。当时在设计的时候集中在了扩展性，可读性和并发性上，多态编程在当时看起来也不是必要的，因此简单起见就被会忽略了。

泛型很方便，但是它也在复杂度、类型系统和运行时方面带来了一些消耗。虽然我们一直在思考，但我们还没有找到一个与复杂性成比例的有价值的设计。与此同时，Go内建的map和slice，加上空接口构造容器的能力，意味着在很多情况下可以编写泛型支持的代码，尽管不太流畅。

这个话题现在仍然没有定论。要查看之前几次不成功的泛型解决方案，可以参看这个[草案](https://golang.org/issue/15292)。

### 为什么Go没有异常处理?

我们认为类似`try-catch-finally`这样的控制结构去耦合异常，会导致代码复杂度提高。这也是鼓励开发者将许多普通的错误标记为异常，例如打开文件失败。

Go采取了一种不同的实现。对于普通的错误处理，Go的多值返回使得报告错误变的容易，而且也不会重载返回值。[A canonical error type, coupled with Go's other features](https://golang.org/doc/articles/error_handling.html)一个规范的错误类型，再加上其他Go的特性，使得和其他语言不同的错误处理也令人愉快.

Go也有一些内置函数，可以在真正发生异常的情况下发出信号并恢复。恢复机制仅在发生错误后函数状态被破坏时执行，这足以处理灾难，但却不需要额外的控制结构，并且要是使用的好的话，可以写出干净的错误处理代码。

细节参见这篇文章[Defer, Panic, and Recover](https://golang.org/doc/articles/defer_panic_recover.html)。也可以看[Errors are values](https://blog.golang.org/errors-are-values)这篇博客，这篇博客描述了Go中一个错误处理的范例，由于错误仅是一个值，所以在错误处理的时候可以用上Go所有的特性。

### 为什么Go没有断言?

Go没有提供断言。不可否认断言很方便，但是根据我们的经验，开发者们有了断言之后就不深入的思考如何去合适的进行错误处理和错误报告。合适的错误处理意味着服务端在遇到一个非致命的错误后还能继续提供服务而不是直接奔溃。合适的错误报告意味着开发者们通过错误可以快速定位问题，从而避免了从大量奔溃信息中跟踪问题。尤其当开发者在不熟悉的代码中遇到了错误，清晰明了的错误是非常重要的。

我们明白这是一个有很多争论的点。在Go语言和标准库中有许多与现代时间实践不同的地方，这仅仅是因为我们认为有时值得尝试另外一种不同的方法。

### 为什么基于CSP理论构建并发模型？

Concurrency and multi-threaded programming have over time developed a reputation for difficulty. We believe this is due partly to complex designs such as [pthreads](https://en.wikipedia.org/wiki/POSIX_Threads) and partly to overemphasis on low-level details such as mutexes, condition variables, and memory barriers. Higher-level interfaces enable much simpler code, even if there are still mutexes and such under the covers.

One of the most successful models for providing high-level linguistic support for concurrency comes from Hoare's Communicating Sequential Processes, or CSP. Occam and Erlang are two well known languages that stem from CSP. Go's concurrency primitives derive from a different part of the family tree whose main contribution is the powerful notion of channels as first class objects. Experience with several earlier languages has shown that the CSP model fits well into a procedural language framework.

### 为什么用goroutines而不是线程?

Goroutines are part of making concurrency easy to use. The idea, which has been around for a while, is to multiplex independently executing functions—coroutines—onto a set of threads. When a coroutine blocks, such as by calling a blocking system call, the run-time automatically moves other coroutines on the same operating system thread to a different, runnable thread so they won't be blocked. The programmer sees none of this, which is the point. The result, which we call goroutines, can be very cheap: they have little overhead beyond the memory for the stack, which is just a few kilobytes.

To make the stacks small, Go's run-time uses resizable, bounded stacks. A newly minted goroutine is given a few kilobytes, which is almost always enough. When it isn't, the run-time grows (and shrinks) the memory for storing the stack automatically, allowing many goroutines to live in a modest amount of memory. The CPU overhead averages about three cheap instructions per function call. It is practical to create hundreds of thousands of goroutines in the same address space. If goroutines were just threads, system resources would run out at a much smaller number.

### 为什么map操作没有没定义为原子操作?

After long discussion it was decided that the typical use of maps did not require safe access from multiple goroutines, and in those cases where it did, the map was probably part of some larger data structure or computation that was already synchronized. Therefore requiring that all map operations grab a mutex would slow down most programs and add safety to few. This was not an easy decision, however, since it means uncontrolled map access can crash the program.

The language does not preclude atomic map updates. When required, such as when hosting an untrusted program, the implementation could interlock map access.

Map access is unsafe only when updates are occurring. As long as all goroutines are only reading—looking up elements in the map, including iterating through it using a `for` `range` loop—and not changing the map by assigning to elements or doing deletions, it is safe for them to access the map concurrently without synchronization.

As an aid to correct map use, some implementations of the language contain a special check that automatically reports at run time when a map is modified unsafely by concurrent execution.

### Will you accept my language change?

People often suggest improvements to the language—the [mailing list](https://groups.google.com/group/golang-nuts) contains a rich history of such discussions—but very few of these changes have been accepted.

Although Go is an open source project, the language and libraries are protected by a [compatibility promise](https://golang.org/doc/go1compat.html) that prevents changes that break existing programs, at least at the source code level (programs may need to be recompiled occasionally to stay current). If your proposal violates the Go 1 specification we cannot even entertain the idea, regardless of its merit. A future major release of Go may be incompatible with Go 1, but discussions on that topic have only just begun and one thing is certain: there will be very few such incompatibilities introduced in the process. Moreover, the compatibility promise encourages us to provide an automatic path forward for old programs to adapt should that situation arise.

Even if your proposal is compatible with the Go 1 spec, it might not be in the spirit of Go's design goals. The article *Go at Google: Language Design in the Service of Software Engineering* explains Go's origins and the motivation behind its design.
