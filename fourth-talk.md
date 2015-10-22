Frederick Kautz
fkautz@pseudocode.cc

Gomez
A Go-like language for embedded systems
https://github.com/fkautz/gomez

What are the current technologies used to implement against embedded systems?
How does Gomez try to improve on this Tech?

## Embedded systems constraints
Strict memory constraints.
No operating system or libc.
No system calls like malloc, open, etc. (You ARE the memory manager, ugh).

Go is very dependent on glibc which provides malloc, etc.

Each arch has it own unique reqs and limits.
- Memory layout
- Entry point
- IO Addresses
- Interrupt handling

Limited or no threads
- Might not have MMU (the lack of this may make running linux impossible)

All of this together creates a very interesting environment to work in. It gets even more interesting when you start to value low power systems.

## Why C?
Basically, a high level abstraction of Assembly.
Multiple mature FOSS compilers:
- GCC
- Clang
Huge ecosystem of libraries.
Easy to find programmers who are familiar.
Highly portable under most conditions.
C is also highly optimized.

## Why Not C?
Too easy to write unsafe code.
You corrupted the memory you fool.
Unsafe pointers & buffer overflows.
- This leads to security concerns.
Unnatural struct to method relationships. This led to OOP eventually, because that makes managing the relationship much simpler.
The C pre-processor further complicates things. It literally allows you to change the language into an ENTIRELY different language.
Example http://www.ioccc.org/

## Why Go?
Can't do obfuscation crazyness.
It's a simpler language to understand.
Simpler to implement.
Fewer surprises.

Go is also nice because it's syntactically simialr to C.
A lot of people already know C! You can pretty much take a C programmer and point them at Go. They're immediatly much more productive.

Safe pointers are in Go, and that means no buffer overflows.
Functions can be safely correlated with data types. (attach functions to structs).

## Why Not Go?
Explicitly not designed for embedded systems. It relies on an OS.
There's also the glibc requirement.

Go also does GC. That brings heap allocation and promotion. This can produce potential latency issues.

These are core features of the Go language, and Go isn't Go without these ideas.

## Why Rust?
Great memory safety.
LLVM support in a fork (it's community based).
Rust is a very complex language.
There can be a very steep learning curve going from C to Rust.

## Goals of Gomez
No OS req.
Portable across architectures.
Simple languae.

(Launches into an example set of code, which is pretty cool as it will compile under both Go and Gomez).

Start with Go and remove features:
RE-use Go's parser, AST, tokens, etc.
Remove heap allocations.
Add filters on top of AST to enforce Gomez rules:
- Remove recursion
- Enforce C-like standards similar to MISRA-C.

Memory is explicitly defined on the stack or in the architecture spec before it's required for use. This allows us to do things like slices, etc.

Innately LLVM supports a lot of architectures.
LLVM already has GREAT optimizers. So you know, rely on that. Optimize for LLVM, not for Gomez.

Goals:
- bring in debugger support.
- Add an architecture spec, which is important for external memory addresses.

(Launches into a CLI example)

LLVM supports a SHIT LOAD of architectures out of the box.

Gomez is still a very new project, and would benefit by community contrubutions! He is doing this in his spare time right now.

Q: What about annotations for things that you don't want to optimize. Volatile for example in C, how would you deal with that in Gomez? Is there anyway to prevent the optimizer from running?
A: The best way to get around the optimization right now is to emit the intermediate representation, but don't run the optimizer.

Would recommend not relying on that too much. LLVM is pretty smart. It knows when something is external, and when something is internal. If you need something that has SUPER strict requirements for timing, etc, then you probably shouldn't be using C nor Go.

Assembly, bro.

Q: Do you know of any projects that are currently relying on Gomez?
A: No! I just open sourced Gomez today. I bought an Arduino, and want to use that to help test Gomez... but also have fun!

The longer term goal is to be able to get Gomez to the point that you could run cars and other critical devices on it. C is fundamentally complex, and tricky. A lot of the time you don't need C, you just need something _like_ C.

Gomez fits that bill!

Q: Is Gomez threadsafe?
A: At the beginning we're going to try and keep Gomez simple. Want to target the lowest common denominator so it works well in the widest array of environments.

Also, threading takes up a lot of power! Which, is why we don't often see threading in embedded systems.
