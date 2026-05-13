# Studio 3

## Control Flow and Debugging

In this studio, you will explore how to track program control flow and corresponding changes to program state using a debugger. You will build, run, and trace a program that implements a simple prefix addition calculator for expressions involving `+` operators and unsigned integers supplied as command-line arguments.
 
The program uses C++ exception handling — throwing, catching, and stack unwinding — to report errors. As you trace the program with a debugger, pay attention to how the call stack behaves differently when an exception is thrown compared to when functions return normally. This will reinforce what you read about exception handling before class.
 
The exercises below use `gdb` in the terminal. Tracing this program with `gdb` will help prepare you to debug program behavior in later labs and studios. Optional enrichment exercises also invite you to trace program behavior with a debugger in Visual Studio Code.

## Collaboration

You may complete this studio individually or in a small group.

## Reference

If you need a refresher on the environment setup steps from the previous studio, see [Studio 0](https://github.com/cse4208-wustl/studio0).

## Exercises

### Part 1: Build and Run

Record your answers in `ANSWERS.md` as you work. Include the names of everyone who worked on the studio in your first answer, and number your responses so they are easy to match to the exercises.

1. List the names of the people who worked together on this studio.

2. SSH into `shell.cec.wustl.edu` using your WUSTL Key credentials, then use `qlogin` to log into one of the Linux Lab machines and confirm that the version of `g++` there is correct, as you did in [Studio 0](https://github.com/cse4208-wustl/studio0).

   Clone your `studio3` repo and work inside that cloned directory.

   The cloned repo already includes `studio3.h`, `studio3.cpp`, and a `Makefile` configured to build an executable program called `studio3`.

   Compile the program, checking that the `Makefile` causes `make` to pass the `-g` flag so debugging information is included for `gdb`.

   Run the compiled program with command-line arguments that form a **prefix addition expression**, where each symbol is separated from the next by a space.

   Examples of valid expressions:

   ```bash
   ./studio3 + 1 2
   ./studio3 + + 1 2 3
   ./studio3 + 1 + 2 3
   ```

   Immediately afterward, confirm that the program returned `0`:

   ```bash
   echo $?
   ```

   Then run the program again with an invalid prefix addition expression. For example:

   ```bash
   ./studio3 + 1
   ```

   Use `echo $?` again to confirm that the program returned a non-zero failure value.

   In your answers, show:

   - the command lines used for the successful and unsuccessful runs
   - the output produced by each run
   - the return value from each run

### Part 2: Basic Debugger Use

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

7. Review the source file and use the `print` command in `gdb` to display:

   - the position index of the current command-line argument being parsed
   - the C-style string corresponding to that command-line argument

   In your answers, show the values that were displayed.

### Part 3: Exceptions and Stack Unwinding

These exercises use the same program and debugger session. They connect what you read about exception handling to observable behavior in the debugger.
 
8. Read through `studio3.cpp` and locate:
   - the two `throw` statements in `parse_and_compute`
   - the two `catch` blocks in `main`
   - the `catch (...)` block
   In your answers, describe in one sentence what condition each `throw` detects, and what exception object is thrown.

9. Set a breakpoint on the line containing `throw Result_Code::unexpected_end_of_expression`:
   ```
   break studio3.cpp:84
   ```
 
   *(Adjust the line number if needed — find the exact line in your editor.)*
   Run the program with an invalid expression that will trigger this exception, for example, one that is missing its final integer:
   ```
   run + 1 + 2 3
   ```
 
   When the breakpoint is hit, display the call stack with `where`. In your answers:
   - Show the full call stack output
   - How many frames of `parse_and_compute` are present?
   - What does that tell you about how deeply the recursion had gone before the throw?

10. Now set a breakpoint at the start of the `catch` block in `main`: the line beginning with `cout << "caught "` (adjust the line number if needed), then continue execution:
    ```
    break studio3.cpp:52
    continue
    ```
 
    When this breakpoint is hit, display the call stack again with `where`. In your answers:
    - Show the call stack output
    - How many frames are present now, compared to exercise 9?
    - This change in the call stack is stack unwinding. In your own words, describe what happened to the intermediate `parse_and_compute` frames between the throw and the catch.

11. While stopped at the `catch` block, print the value of the exception object:
    ```
    print rc
    ```
 
    In your answers, show the printed value. What type is `rc`, and how does the program use its value after catching it?

12. **Normal return vs. exception unwinding.** Restart the debugger with a valid expression. Set a breakpoint on the line `return first_operand + second_operand` in `parse_and_compute`. Each time it is hit, use `where` to observe the call stack, then `continue` to the next hit.
    In your answers, describe how the call stack changes across these successive breakpoint hits. How does this frame-by-frame shrinking compare to what you observed in exercise 10, where all the intermediate frames disappeared at once?


13. **The `catch (...)` block.** Look at the `catch (...)` block in `main`. In your answers:
    - What kinds of exceptions would reach this block but not the `catch (Result_Code rc)` block above it?
    - Why might it be good practice to include a `catch (...)` even when you believe you know every exception type that could be thrown?

## Optional Enrichment

1. Use the `step` and `next` commands within `gdb` to step into and step over different statements and functions in the program.

   In your answers, note any interesting observations, including whether you were able to step all the way down into assembly code for functions that come from the C++ libraries rather than the program's own source.

2. Repeat any of the required or optional exercises using a debugger from VS Code. Your repository contains a .vscode directory with the configurations this debugger will need.

   In your answers, describe briefly what you did and any useful features VS Code provided when debugging the program.

## Deliverables

Commit and push all modified and added files to the repo.
