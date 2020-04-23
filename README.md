Cram test demo
==============

Preparation
-----------

First, you need to build the dune-cram tool. It is currently part of
dune and not yet extracted as a standalone project. So you need to go
in the dune source code and run:

    $ dune build otherlibs/cram/bin/dune_cram.exe

You can then copy the tool to your bin directory as follow:

    $ cp _build/default/otherlibs/cram/bin/dune_cram.exe ~/bin

Assuming `~/bin` is in your `PATH` variable, you can go ahead with
setting up the test. For that, create a new directory for the demo
with a dune file containing:

```
(rule
 (alias runtest)
 (action
  (progn
   (run dune-cram run run.t)
   (diff? run.t run.t.corrected))))
```

Demo
----

Create a `run.t` file containing:

```
  $ cat >hello.ml <<EOF
  > print_endline "Hello, world!"
  > EOF

  $ ocamlc hello.ml

  $ ./a.out
```

The syntax of `.t` file is as follow:

- lines starting with two spaces and a dollar sign are shell commands
- lines starting with two spaces are the output of the shell commands
- everything else is a comment

The nice thing about this syntax is that it is very close to what you
would write and observe in your terminal.  Making cram tests very easy
to learn and work with.

In the example above, we are missing the output of the `./a.out`
command. We will now ask dune to test this `run.t` file. To do that,
simply run:

    $ dune runtest

The `dune-cram` tool executed by `dune runtest` will execute the shell
commands, capture their outputs and produce a new updated
`run.t.corrected` file. And because the updated version is not the
same as the original since the output of `./a.out` was missing, dune
displays a diff between the two files.

If the new version looks good to you, you can tell dune to replace the
`run.t` file by the new one in place by running:

   $ dune promote

If you use emacs and have installed the dune mode, you can also do
`M-x dune-promote` inside the `run.t` file. This will promote the new
version and also update the emacs buffer. The editor integration is
quite nice as it gives ome kind of interactive experience: write shell
commands, use dune to evaluate them and accept the result. It is even
possible to automate all these steps by using the dune watch mode with
`--auto-promote` and setting up emacs to automatically reload files.

This is a test!
---------------

Now that we have an up to date `run.t` file, we can drop this file and
the `dune` file in our test suite and `dune runtest` will continue to
execute it. If anything change that would break the test, we would get
a nice diff showing us the change in behaviour.



