---
sidebar_position: 2
slug: /docs/tutorial/hello-world
---

# Your First Luvit App

In this tutorial, we will be creating a simple Hello World app that runs in the Luvit environment. By the end of it, this application will have become a (very simple) webserver that responds to your requests with the same "Hello world" text, can be run like any ordinary executable completely on its own, and is turned into a self-contained Lit package ready to be uploaded and shared with the world.

## Creating Your Application

Luvit apps adhere to a specific structure when they are turned into packages, but for now you can simply create a regular lua script:

```lua title="HelloWorld.lua"
print("Hello world")
```

This script could also be run in a standard ``lua`` interpreter, as it doesn't make use of the Luvit standard library.

## Running the Application

So far, this seems pretty boring. Running the script by typing ``luvit HelloWorld.lua`` will yield the following output:

> Hello world

... and after this, your program exits. This is exactly what you'd expect. Let's make things interesting and access the global environment.

## The Luvi Environment

In order to explore something you *can't* usually do in standard Lua, we now modify the script to import ``uv`` from the Luvit ecosystem:

```lua title="HelloWorld.lua"
local uv = require("uv")

print("Hello world")

local SLEEP_TIME_IN_MILLISECONDS = 3000
uv.sleep(SLEEP_TIME_IN_MILLISECONDS)

print("Good morning!")
```

Running the script again should now print

> Hello world

before waiting three seconds, printing

> Good morning!

and finally exiting.

The ``uv`` library allows us to call any function provided by the [``libuv``](http://docs.libuv.org/en/v1.x/api.html) C-library from Lua. This makes it possible to do a variety of things Lua can't normally do, such as accessing the file system, creating threads, setting timers, or sending network messages. All this is automatically included in the ``luvit`` executable (and, in fact, even inside the ``luvi`` runtime itself) so we can always ``require`` it.

Before we focus on the ``uv`` library and what else it can do, there's something very important that we can learn from the above:

**The Luvit environment is not the same as a standard Lua environment!** It has all the regular Lua 5.1 functions, like ``print`` and ``dofile``, but as we have seen above it also includes certain globals that aren't usually present.

These are injected into your application by the ``luvi`` runtime, and can be roughly sorted into two categories:

* Nonstandard libraries, like ``uv``, ``jit`` (LuaJIT API), ``openssl`` (OpenSSL API), or ``miniz`` (compression library)
* Special "magic" globals, like ``process`` (to access the current application itself) and ``p`` (pretty-print shorthand)

One thing to keep in mind here is that only the libraries are included in the ``luvi`` runtime, but magic globals are only added when executing your script via the ``luvit`` runtime. If you want them as part of a self-contained package (which is what we'll be doing later), you'll have to ``require`` them first. (**TODO: Move to the respective part? It's too early here, and might be confusing/TMI**)

If you aren't sure what functionality exists, simply check the API documentation.

- p





However, since this isn't very convenient Luvit ships with a set of higher-level standard libraries that abstract away this functionality and build on it to provide easier-to-use interfaces.

## Relative Imports

- require

## Introducing the Event Loop

Possibly the most important part of the Luvit environment is the event loop, which is closely linked to libuv and the ``uv`` library.