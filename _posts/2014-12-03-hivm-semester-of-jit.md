---
layout: post
title:  "Hivm: semester of JIT"
date:   2014-12-03 20:00:00
---

This fall semester I had the opportunity to do a self-directed independent study in tracing just-in-time (JIT) compilation. The goals for the study were to develop a working instruction tracer, trace-directed compiler, and (time permitting) exploration of basic optimizations. I'm pleased to say that all of those goals were achieved! What follows is a recap of the past fews months' efforts in building a JIT compiler on top of the experimental virtual machine [Hivm](http://hivm.org/). A brief run-down of [the numbers]:

[the numbers]: https://github.com/dirk/hivm/compare/42f09c047b0966f1bd9856f76ee7f7ec647b5a1c...master

* 136 commits
* 34 files changed (12 of those new files)
* 4,104 additions
* 778 deletions
* Countless tabs of LLVM C API documentation and runs of `lldb`/`gdb`

Let's now dig into some of the specifics of this journey.

#### Laying the groundwork

August was dedicated to building a foundation of tools for developing the JIT compiler. This included embedding a Lua runtime that could be launched as an interactive debugging session (inspired by [GDB] and the like). Within this debugging environment you can call functions to perform [stack traces], [set breakpoints], and [inspect registers].

The Lua project has achieved something special: a language and virtual machine that are very, very easy to embed and work with. It only took about 25 lines of C to hook everything together for a rudimentary-yet-usable interactive debugger, and most of that was just glue code to expose C functions to the Lua environment.

[GDB]: http://www.gnu.org/software/gdb/

[stack traces]: https://github.com/dirk/hivm/blob/0d319caf4bd4a773a5cded72501da013dbc58b05/src/debug-lua.include.c#L21-L40

[set breakpoints]: https://github.com/dirk/hivm/blob/0d319caf4bd4a773a5cded72501da013dbc58b05/src/debug-lua.include.c#L42-L58

[inspect registers]: https://github.com/dirk/hivm/blob/0d319caf4bd4a773a5cded72501da013dbc58b05/src/debug-lua.include.c#L60-L72

### Tracing

September was devoted to building the tracing infrastructure. Most JIT compilers are driven by *traces*, which are essentially live recordings of the execution of a section of code. This approach lets the virtual machine (VM) get a detailed view of the exact operation of a particular code path; it also lets it choose which code path to watch. By only tracing and compiling "hot" (frequently taken) code paths, a VM can achieve an effective balance of responsiveness and performance.

Compiled native code is much faster than interpreted bytecode or AST-walking, however generating that native code is expensive. In a traditional compiler you have do a **bunch of stuff** to build an executable: parse the source, resolve and check types, emit code, link objects, and so on. This takes a time. With a JIT all of that is deferred until absolutely necessary, and only done for the sections of code that are executed a lot. This deferral removes the need for complicated, time-consuming compilation of programs before they can be executed.

The other neat feature of tracing JIT compilation is that the trace knows *exactly* what's going on in the program. In traditional static compilation the compiler must figure out what is going on: it has to resolve types, compute array/structure/class/etc. indexes and lookups, and a bunch of other heuristics. On the other hand, when tracing a program all these types, structures, and so forth are revealed to us by the program itself! The machine doesn't have to do complicated logic to figure out what a variable is going to look like, instead when it's tracing the program's execution the variable is sitting right there to be examined.

Hivm's tracer works by injecting itself into the [fetch-decode-execute] loop and watching each instruction as it's fetched and decoded. As it does this it builds up a trace of structured pieces of data recording each instruction that the machine executed. Under the hood there are **two** separate [dispatchers] doing the fetching, decoding, and executing. Similar to a dual-clutch gearbox, one dispatcher is used when not-tracing and the other is used when it is tracing. Naturally, the former is the active dispatcher most of the time, however when it is necessary to trace a code path the machine seamlessly switches over to the tracing dispatcher. Once the tracing dispatcher has finished its traces it seamlessly switches back to the faster non-tracing dispatcher.


[fetch-decode-execute]: http://en.wikipedia.org/wiki/Instruction_cycle

[dispatchers]: https://github.com/dirk/hivm/blob/0d319caf4bd4a773a5cded72501da013dbc58b05/src/vm-dispatch.include.c

### Compilation

The compiler takes a given trace and constructs native code based off of it. For assembling native code it uses the C API of the [LLVM compiler infrastructure](http://www.llvm.org/). At a high level the compilation process is fairly straightforward: it merely generates the native code equivalent to what the VM dispatcher would be doing. For basic stuff like call subroutines, doing math, and so forth this is simple.

However, some operations require a bit more work. The first is the handling of control flow and bailouts. The structuring of control flow (at its most basic level just if-then-else and jump) in Hivm is based on instructions directing the VM to change address (program counter/instruction pointer in technical terms). Is something true? If yes then go to this point in the instruction sequence, otherwise just execute the next instruction. LLVM is slightly different: control flow is modeled after "basic blocks". Imagine a piece of C-like code; see all those brackets? Each of those brackets is a block in LLVM's view, and control flow is going between those blocks. There is no go-to-a-specific-instruction in LLVM, instead there is go-to-a-block. For this reason the JIT compiler has to do [a bit of finagling][1] to turn the linear sequence of traced instructions into a set of discrete LLVM blocks. It does so by identifying all the different possible branch points and their associated destination points, and then splits into blocks based on those points. It's a bit hacky, pretty nifty, and thankfully seems to work.

[1]: https://github.com/dirk/hivm/blob/0d319caf4bd4a773a5cded72501da013dbc58b05/src/jit-compiler.c#L1068-L1141

The other tricky thing is the handling of bail-outs. In some cases the JIT code may run into a situation within a piece of compiled code where it doesn't have the code to handle what's happening. Examples of this include an if-condition where one branch was not taken in the trace (and therefore wasn't compiled) or some sort of exception happened (out of bounds, type error, etc.). In these situations the compiler inserts a bit of code which literally "bails out" of the compiled native code back to the matching point in the VM. In the process it extracts all the compiled version of the objects that would be in the registers and inserts them into their matching VM registers. This way the VM can seamlessly pick up where the JIT left off.

Work has also begun on basic optimizations. Currently the JIT inlines known constant values into instructions, and soon it will inline local variables as well. It also leans heavily on the powerful set of optimization tools provided by LLVM.

### Conclusion

This was by far my favorite school-associated work. For years I'd day-dreamed about writing a JIT compiler, so this was—to be a bit cliché—a **dream come true**. It was a joy to put into practice all the things I had read about JIT design and also learn some new lessons about the intricacies of compiler design and effective instruction tracing. In the near future the Hivm project should see the rest of those small inlining optimizations in the JIT, refactoring of some instruction syntaxes, and the beginnings of the first full-fledged language to run on it.
