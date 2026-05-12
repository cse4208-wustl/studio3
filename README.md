# Studio 3

## Control Flow and Debugging

In this studio, you will explore how to track program control flow and corresponding changes to program state using the `gdb` debugger. You will build, run, and trace a program that implements a simple prefix addition calculator for expressions involving `+` operators and unsigned integers supplied as command-line arguments.

Tracing this program with `gdb` will help prepare you to debug program behavior in later labs and studios. Optional enrichment exercises also invite you to trace program behavior using `gdb` inside `emacs` or with the Visual Studio debugger on lab machines.

## Collaboration

You may complete this studio individually or in a small group.

## Reference

If you need a refresher on the environment setup steps from the previous studio, see [Studio 0](https://github.com/cse4208-wustl/studio0).

## Exercises

Record your answers in `ANSWERS.md` as you work. Include the names of everyone who worked on the studio in your first answer, and number your responses so they are easy to match to the exercises.

1. List the names of the people who worked together on this studio.

2. SSH into `shell.cec.wustl.edu` using your WUSTL Key credentials, then use `qlogin` to log into one of the Linux Lab machines and confirm that the version of `g++` there is correct, as you did in [Studio 0](https://github.com/cse4208-wustl/studio0).

   Clone your `studio3` repo and work inside that cloned directory.

   The cloned repo already includes `studio3.h`, `studio3.cpp`, and a `Makefile` configured to build an executable program called `studio3`.

   Compile the program, checking that the `Makefile` causes `make` to pass the `-g` flag so debugging information is included for `gdb`.

   Run the compiled program with a valid prefix addition expression containing multiple space-separated `+` operators and unsigned integers. There should be one more unsigned integer than the number of addition operators, and the *i*th addition operator should appear to the left of the *i*th unsigned integer.

   Immediately afterward, confirm that the program returned `0`:

   ```bash
   echo $?
   ```

   Then run the program again with an invalid prefix addition expression, for example by removing one unsigned integer or by placing an unsigned integer before its corresponding addition operator. Use `echo $?` again to confirm that the program returned a non-zero failure value.

   In your answers, show:

   - the command lines used for the successful and unsuccessful runs
   - the output produced by each run
   - the return value from each run

3. Run:

   ```bash
   gdb studio3
   ```

   In your answers, show the lines that appeared in the terminal when you loaded the program in `gdb`.

4. Within `gdb`, set a breakpoint at `parse_and_compute`:

   ```gdb
   break parse_and_compute
   ```

   In your answers, show the additional lines printed in the terminal when you ran that command.

5. Within `gdb`, run the program and continue to the next breakpoint:

   ```gdb
   run + 1 + 2 + 3 4
   continue
   ```

   In your answers, show the additional lines printed in the terminal when you ran those commands.

6. Within `gdb`, display the current call stack:

   ```gdb
   where
   ```

   In your answers, show the additional lines printed in the terminal when you ran that command.

7. Review the source file or the lecture slides to determine:

   - the expression for the position index of the current command-line argument being parsed
   - the expression for the C-style string corresponding to that command-line argument

   Then, within `gdb`, use the `print` command twice to display those values. In your answers, show the additional lines printed when you did that.

## Optional Enrichment

1. Use the `step` and `next` commands within `gdb` to step into and step over different statements and functions in the program.

   In your answers, note any interesting observations, including whether you were able to step all the way down into assembly code for functions that come from the C++ libraries rather than the program's own source.

2. Repeat any of the required or optional exercises after opening the source and header files in `emacs` and running `gdb` from within `emacs`, as shown in the lecture slides.

   In your answers, describe briefly what you did and any useful features `emacs` provided while running `gdb`.

3. Repeat any of the required or optional exercises after transferring the program source and header files to a Windows lab machine and stepping through the program with Visual Studio's debugging features.

   In your answers, describe briefly what you did and any useful features Visual Studio provided when debugging the program.

## Deliverables

Commit and push all modified and added files to the repo.
