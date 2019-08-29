### How are libraries documented?

There is a program, `godoc`, written in Go, that extracts package documentation from the source code and serves it as a web page with links to declarations, files, and so on. An instance is running at [golang.org/pkg/](https://golang.org/pkg/). In fact, `godoc` implements the full site at [golang.org/](https://golang.org/).

A `godoc` instance may be configured to provide rich, interactive static analyses of symbols in the programs it displays; details are listed [here](https://golang.org/lib/godoc/analysis/help.html).

For access to documentation from the command line, the [go](https://golang.org/pkg/cmd/go/) tool has a [doc](https://golang.org/pkg/cmd/go/#hdr-Show_documentation_for_package_or_symbol) subcommand that provides a textual interface to the same information.

### Is there a Go programming style guide?

There is no explicit style guide, although there is certainly a recognizable "Go style".

Go has established conventions to guide decisions around naming, layout, and file organization. The document [Effective Go](https://golang.org/doc/effective_go.html) contains some advice on these topics. More directly, the program `gofmt` is a pretty-printer whose purpose is to enforce layout rules; it replaces the usual compendium of do's and don'ts that allows interpretation. All the Go code in the repository, and the vast majority in the open source world, has been run through `gofmt`.

The document titled [Go Code Review Comments](https://golang.org/s/comments) is a collection of very short essays about details of Go idiom that are often missed by programmers. It is a handy reference for people doing code reviews for Go projects.

### How do I submit patches to the Go libraries?

The library sources are in the `src` directory of the repository. If you want to make a significant change, please discuss on the mailing list before embarking.

See the document [Contributing to the Go project](https://golang.org/doc/contribute.html) for more information about how to proceed.

### Why does "go get" use HTTPS when cloning a repository?

Companies often permit outgoing traffic only on the standard TCP ports 80 (HTTP) and 443 (HTTPS), blocking outgoing traffic on other ports, including TCP port 9418 (git) and TCP port 22 (SSH). When using HTTPS instead of HTTP, `git` enforces certificate validation by default, providing protection against man-in-the-middle, eavesdropping and tampering attacks. The `go get` command therefore uses HTTPS for safety.

`Git` can be configured to authenticate over HTTPS or to use SSH in place of HTTPS. To authenticate over HTTPS, you can add a line to the `$HOME/.netrc` file that git consults:

```shell
machine github.com login USERNAME password APIKEY
```

For GitHub accounts, the password can be a [personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

`Git` can also be configured to use SSH in place of HTTPS for URLs matching a given prefix. For example, to use SSH for all GitHub access, add these lines to your `~/.gitconfig`:

```
[url "ssh://git@github.com/"]
	insteadOf = https://github.com/
```

### How should I manage package versions using "go get"?

Since the inception of the project, Go has had no explicit concept of package versions, but that is changing. Versioning is a source of significant complexity, especially in large code bases, and it has taken some time to develop an approach that works well at scale in a large enough variety of situations to be appropriate to supply to all Go users.

The Go 1.11 release adds new, experimental support for package versioning to the `go` command, in the form of Go modules. For more information, see the [Go 1.11 release notes](https://golang.org/doc/go1.11#modules) and the [`go` command documentation](https://golang.org/cmd/go#hdr-Modules__module_versions__and_more).

Regardless of the actual package management technology, "go get" and the larger Go toolchain does provide isolation of packages with different import paths. For example, the standard library's `html/template` and `text/template` coexist even though both are "package template". This observation leads to some advice for package authors and package users.

Packages intended for public use should try to maintain backwards compatibility as they evolve. The [Go 1 compatibility guidelines](https://golang.org/doc/go1compat.html) are a good reference here: don't remove exported names, encourage tagged composite literals, and so on. If different functionality is required, add a new name instead of changing an old one. If a complete break is required, create a new package with a new import path.

If you're using an externally supplied package and worry that it might change in unexpected ways, but are not yet using Go modules, the simplest solution is to copy it to your local repository. This is the approach Google takes internally and is supported by the `go` command through a technique called "vendoring". This involves storing a copy of the dependency under a new import path that identifies it as a local copy. See the [design document](https://golang.org/s/go15vendor) for details.