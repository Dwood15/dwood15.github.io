---
layout: post
title: First Pass at learning Zig
---

Under a haze of hubris and the fumes of "what if", I wanted to implement a "toy" pattern for randomizer. While doing some
basic research on the subject, I've discovered that the OOT-Randomizer is written effectively-entirely in python, and 
is quite difficult to wrap a newb's head around. I wanted to take some extra time, pull out some parts, and write it in
a language with static types. 

Why Zig? I wanted to venture into languages with a unique take on memory management. I've been programming in Golang the
most over the last year or so, and while I could probably get up and running in Golang super fast, I don't want to stay
in only one spot, language-wise, for my whole career. Therefore, it's _time to explore_!

---

# Getting set up 

## Installing Zig

I nabbed the appropriate binaries for my machine (linux amd64) off [the main site](https://ziglang.org/download/), and added
them to my path.
 
 When I run `zig version` in bash, I get: `0.5.0`, we're good for now. 

## First Program 

Zig promises some level of simplicity, and easy compatibility with C. `zig init-lib`. I modify the build.zig file to 
load the generated shared object file: 

In my build file, I change the line under `b.standardReleaseOptions();`, like so: 

```Zig
    const mode = b.standardReleaseOptions();
    const lib = b.addSharedLibrary("OoT-Randomizer", "src/main.zig", b.version(0, 0, 1));
```

`zig build` does something without errors or messages. Easy.

###First hitch

- tl;dr - The shared library is stuffed into: `./zig-cache/lib`. and is 3 file names, all sym-linked to the most specific 
version.

- generated file name: `libOoT-Randomizer.so.0.0.1` (yeeeaaaa...)

## Integrating with the OoT-Randomizer

1) In the repository, move the python project to a sub-folder. This is kind of awkward for now, but that's OK. This is 
less about OoTR than trying to learn zig. 

2) Nuke the entire electron Gui project and Gui.py - where we're going, we _don't need_ gui. 

3) Go to `OoTRandomizer.py` and perform some minor cleanup. (remove references to the gui, for ex)
    - call `python OoTRandomizer.py` and fix any compiler errors (usually the missing gui, those refs should be removed)

4) In `OoTRandomizer.py`, reference-and-load the generated shared-library.

```Python
from ctypes import *
zig_lib = "../zig-cache/lib/libOoT-Randomizer.so

def get_zig_funcs():
    zig_funcs = CDLL(zig_lib)
    print(type(zig_funcs))

    res = zig_funcs.add(1, 2)
    print("add res: ")
    print(res)
    print("num_args: ")
    print(zig_funcs.num_args())
    return zig_funcs
```

I'm getting lazy, and wanting to move to where I currently am, but effectively call this func in start() and pass
this to all the functions where you want to call the shared lib.

### Second hitch

In `/src/main.zig`, I used the basic add func as a test. This works in python. we get 3 back, just fine. But what if
we wanted to do something more complicated, such as get and parse application args? The num_args() func I added: 

```Zig
func num_args() i32 {
    return @intCast(i32, os.argv.len);
}
```
Returns zero, no matter what! Hm. I'm not one for speculation, but I'd say that os.argv is only loaded from an exe.

[std.os.argv](https://ziglang.org/documentation/master/std/#std;os.argv) Seems to agree - it's not even available on windows.

Now what? Well, some searching leads me to [std.process.args](https://ziglang.org/documentation/master/std/#std;process) 

But to use that, I need an allocator! Let's just copy-paste the [one from documentation](https://ziglang.org/documentation/master/std/#std;process).
 
```Zig
    var arena = std.heap.ArenaAllocator.init(std.heap.page_allocator);
    defer arena.deinit();

    const a = &arena.allocator;

    var args = std.process.args();

    const firstArg = try (args.next(a) orelse {
	std.debug.warn("Expected an argument to exist");
	return 0;
    }));

    return @intCast(i32, os.argv.len);
```

Easy, right? Wrong! Trying to build the library after naively plugging that together yields: 

```Zig
/home/dwood/Documents/github/OoT-Randomizer/src/main.zig:9:54: error: container 'std.heap' has no member called 'page_allocator'
    var arena = std.heap.ArenaAllocator.init(std.heap.page_allocator);
```

Lol, what?

That's copypasta right from the main documentation example! 

---

Turns out, "master" branch is updated _a lot_, and instead of looking at latest, I needed to look at [the versioned](https://ziglang.org/documentation/0.5.0) docs.
My local version is already out of date with master!

On my local, the "direct_allocator" is what I needed - "page_allocator" was brand new as of the time of this post.

After that, it compiled. Calling the new function in the Python code still doesn't work, but we're making progress, right?

### Third hitch 

How the heck is error handling supposed to work? (Hint: Only the most common cases are pleasant.)

####To Be Continued....


