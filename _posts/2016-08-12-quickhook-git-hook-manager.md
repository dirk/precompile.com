---
layout:  post
title:   "Quickhook: a new Git hook manager"
date:    2016-09-21 21:15:00
summary: A faster, simpler Git hook runner.
---

Git hooks are a useful tool: they're great for automating checking up behind yourself. There are already some great hook managers out there, such as [Overcommit][]. I like what they do, but I have some disagreements with the various technical details of their approaches.

[Overcommit]: https://github.com/brigade/overcommit

Over the past couple of weeks I built [Quickhook][]. It's a hook runner designed to be fast, self-contained, and Unix'y. Hooks are just programs in the hook directory. Quickhook manages running those programs, collecting exit codes and outputs, and reporting that to Git.

[Quickhook]: https://github.com/dirk/quickhook

As an example, the pre-commit hook output in the Quickhook repository looks like:

```sh
$ git commit -m "Make directory existence check more precise"
go-vet: ok
trailing-whitespace: ok
[master 910179f] Make directory existence check more precise
 2 files changed, 14 insertions(+), 11 deletions(-)
```

But if you try to commit some code that fails the `go vet` check it will report the failure. It also exits with a non-zero exit code, which stops Git from proceeding with the commit.

```sh
$ git commit -m "Some incorrect code"
go-vet: fail
context/context.go:97: unreachable code
```

"Manager" isn't the right word for Quickhook. Runner or coordinator is a better description. It has no knowledge of the implementation of the hook files. You can write hooks in Bash, Ruby, Javascript, or even compiled executables. And all you need in the `.git/hooks/pre-commit` script is a call to `quickhook hook pre-commit`. Go check it out [on GitHub](https://github.com/dirk/quickhook), and you install it [via a Homebrew tap](https://github.com/dirk/homebrew-quickhook).
