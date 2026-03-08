# CSE110A: Fundamentals of Compiler Design

## Homework 5: Optimizations

# Preliminaries

1.  Start early! There are lots of corner cases to handle in this
    assignment. You should make plenty of use of the python debugger
    (`pdb`).

2.  There is no report part of this assignment. Please document your
    code thoroughly.

3.  This assignment will be graded as a whole. This document describes
    how you can split up the work to make it more manageable, but your
    grade will be based on a final program that can take test programs
    as an input and provide **optimized** ClassIR programs as an output
    that can be compiled and executed.

4.  You are encouraged to use your HW 4 solution in this assignment.
    However, we are providing a baseline HW 4 solution in case you are
    not confident in your solution.

# File Overview and Getting Started

For this assignment, you will implement local value numbering and a very
simple version of loop unrolling.

The skeleton provided is almost identical to Homework 4. There are two
new test cases designed to help you debug your optimizations.
Additionally, there is a new file for your local value numbering code
(`local_value_numbering.py`).

## Getting started

If you want to use your HW 4 solution, please copy over your completed
`parser.py` and `ast.py` from homework 4 into `cse110A_parser.py` and
`cse110A_ast.py`.  Save the the ones we gave you just in case you change
your mind.    The only change to the API is that the `parse`
function now takes an extra argument: `unroll_factor`, i.e. the number
of times to unroll the loop.

You should be able to run the test cases again to verify that you have
correctly applied the changes.

Keep in mind that you can run the compiler outside of the testing script
for any of the tests simply by running:

```bash
python3 main.py tests/test0/test0.cpp
```

This way of running the program makes it easier to debug with print
statements or with the python debugger (pdb).

If you want to fully test that the results are correct do this:

```bash
python3 main.py tests/test0/test0.cpp > tests/test0/test0ir.cpp
cd tests/test0
make
./original
./compiled
cd -
```
The results from running ./original and ./compiled should match.

# Loop unrolling

The `main.py` now takes an additional argument: `-uf`: the unroll factor
(defaults to 1). Given this argument, it will pass it to the `parse`
function of the parser. It is your job to unroll all loops that you can
by this value.

## Analyzing loops

We will consider a simplified version of loop unrolling in this
assignment. We will unroll loops with the following properties:

1.  There are no branches inside of the loop body (otherwise each unrolled
    body would require generating fresh labels).

2.  The loop iterations can be determined by the compiler, and the
    unroll factor it will evenly divide the loop iterations it it cannot
    cleanup loops will be required.

3.  We will choose to limit the use of loop unrolling to when loop iterations 
    that use simple increment addition updates and a less-than condition 
    (otherwise the arithmetic gets a little complicated).

You can implement loop unrolling by performing the following analysis
when parsing and creating 3-address code in a for loop statement:

1.  identify the iteration variable. This variable will be the lhs of
    the first assignment statement in the for loop.

2.  determine if the iteration variable is assigned a constant integer
    in the assignment statement.

3.  determine if the condition expression is a less-than expression with
    the iteration variable on the lhs and an integer constant on the
    rhs.

4.  determine if the update assignment expression updates the iteration
    variable by adding the value 1 (i.e. it is an increment).

5.  check if the loop body has any control flow.

6.  check if the loop body updates the iteration variable as part of an
    assignment statement.

7.  check that the unroll factor evenly divides the number of loop
    iterations

You will need to analyze all for loops. If they pass those conditions,
then you should unroll them by the unroll factor (-uf) following the method
shown in the lecture. You are welcome to implement more aggressive loop
unrolling, e.g. by adding in cleanup loops, or analyzing more complex
loops with different updates. However, make sure that your compiler
emits code with the same behavior as the input program.

## Implementation

You should be able to implement it completely in `cse110A_parser.py`,
specifically in the function that parses the for loop statements. You
can check your implementation on test7, which can be unrolled by factors
that are a power of 2 (up to 1024). You should also test your code when
the loops are not able to be unrolled, e.g., by running test7 with
unroll factor of 3.

# Local Value Numbering

Your next task is to implement local value numbering over a list of
ClassIR code. The interface for this is given in `ir_compiler.py`. Note
that after the program is parsed, it is sent to the LVN method of
`local_value_numbering.py`. This method takes in a list of ClassIR code
and returns three items:

- a new list of ClassIR code that has been optimized with the local
  value numbering optimization

- a list of new variables that the optimized code requires

- a number stating how many arithmetic operations it was able to replace
  with copy instructions

The new code is then printed out, along with the new variable
definitions and a comment at the top of the file documenting how many
instructions were replaced.

The optimization can be toggled with the command line flag to main:
`-lvn` (default off). The test script does not have this flag on by
default. As you start testing, you should add the flag to the testing
script (or simply run the compiler outside of the testing script).

## Technical details

You should only number virtual registers. These can either be the
virtual registers that were assigned when parsing (e.g. they probably
start with the prefix `vr`), or program variables that were compiled
into virtual registers (e.g. they probably start with the prefix
`_new_name`).

Because of this, you should only consider replacing instructions that
take virtual registers.

You should assume commutativity of integer addition, multiplication and
equality, but no other operators. You do not need to do any sort of
constant propagation or folding. You do not need to do any copy folding
or copy propagation. You do not need to encode any arithmetic
identities. However, you can implement these simple extensions if you’d
like to.

Every time you replace an instruction that eliminates an operation you 
should increment a counter then return the cumulative counter value from 
the top level LVN function. You should track any virtual registers you 
need and return them through a list from the top level LVN function. Also
return the new optimized IR program.  What to return is shown in the 
local_value_numbering.py template given to you.  You can have unlimited 
registers as long as you declare them.

Your implementation for LVN optimization should be constrained entirely to
`local_value_numbering.py`, but you can write as much code in that file
as you’d like (including making new functions or classes).

## Expected outcome

You have a lot of freedom on how to implement local value numbering. We
will be grading your work on if it is able to identify simple
replacements. For example, the optimized version of Test 6 should only
have one `addi` operation.

## Suggested path

While local value numbering is conceptually a simple optimization, it
can be tricky to implement. There is a decent amount of code required to
parse[^1] the different ClassIR instructions. As a rough estimate, my
solution file is close to 200 lines of Python (although it is not as
concise as it could be)

Here is the path I suggest you take, along with ways how to test it at
each step.

### Splitting the code into basic blocks

The LVN function takes in a list of three address code. The simplest
place to get started is to split that list into basic blocks. To do
this, you will need to do some simple parsing of ClassIR code. Given
the simple nature of ClassIR, Python’s regular expressions should be
sufficient.

After the code is split into basic blocks, you should be able to merge
it into a single program again (i.e. a list of 3 address code) and
return it from LVN and all the tests should still pass.

### Numbering

Next I suggest you iterate over the basic blocks and perform the
numbering. This will require finer-grained parsing of ClassIR
instructions, however this is still possible with Python regular
expressions. For example to parse a two-operand ClassIR instruction,
you can use the regular expression:

```python
r"(\\S+)=(\\S+)\\((\\S+),(\\S+)\\)"
```

If the instruction matches, then a match object is returned. Index 0
will contain the whole match, index 1 will contain the destination
register, index 2 will contain the instruction, index 3 and 4 will
contain the arguments.

As a hint, I would strip away any whitespace in your instructions before
doing this regular expression matching. I suggest numbering variables
with an underscore (\_) and a global counter. Recall that you should
only number virtual registers. These are variables that start with `vr`
or `_new_name`.

Make sure to track any new registers you need while numbering and return
them from the top level LVN function.

To test this part of the assignment, you can take these numbered basic
blocks and merge them back into a single program and return it. If
you’ve done this part correctly, the test cases should compile (with
clang), but they may not execute correctly (e.g. the test cases with
loops might loop indefinitely).

### Patching 

Next I suggest you patch the numbered basic blocks so that they will
correctly execute. The simplest way to do this is to modify the start
and end of the basic block. At the end, the original variables should be
assigned to their latest numbered variant. And at the beginning, the
earliest numbered variants should be assigned to the original variables.
For example, given this numbered code:

```text
a2 = b0+c1;
```

it should be patched to:

```text
b0 = b;
c1 = c;
a2 = b0+c1;
a = a2;
```

Of course you will be working with ClassIR code. Pay special attention
where you add the new code, especially if your basic block starts with a
label or ends with a branch.

If you’ve done this correctly, then the merged code should now execute
all the test cases correctly with the numbered basic blocks.

### Optimizing

Now perform the local value optimization on the numbered code. Simply
parse any 2 operand instruction and try to identify places where
arithmetic instructions can be replaced with copy instructions.

Any time you can replace an arithmetic instruction, you should increment
a counter and return it from the top level LVN function. This will leave
a comment at the top of the file with how many arithmetic operations
were replaced. For example in Test 6 you should see that 1 operation was
replaced. If you run test 7 with an unroll factor of 512, you should see
that 1023 instructions were replaced.

# Seeing the effect of optimizations

I have provided a timing script: `time.sh` which you can run. It runs a
slightly modified version of test 7, which iterates more times so as to
get more reliable timing information.

It runs the experiment 4 times:

- once without any optimizations

- once with only local value numbering

- once with a large unroll factor (512)

- and once with both local value numbering and the large unroll

For each, the time in milliseconds is output. You should be able to
observe interesting speedups (and potentially slowdowns). You should be
able to reason about your observations and justify why they occur.

# Extras

If you finish the assignment and want to do more, consider applying more
optimizations to the local value numbering, e.g. constant or copy
propagation/folding. If you add extras, you can feel free to share other
timing experiments or report on how much you were able to additionally
improve the speed of `time.sh`.

# What to submit

There is no report for this assignment. However, please be thorough in
your code documentation.

Your work should be completed in `cse110A_parser.py` and
`local_value_numbering.py`. It should be possible for us to use your
modified files into a fresh skeleton and to pass the tests. Please make
sure you submit the required files to both Gradescope and Github repo.

# Submitting to GitHub Classroom

As mentioned in the introduction, signing up for the GitHub Classroom
assignment will automatically create a repository for you under the
`ucsc-cse110a` organization. This repository is for you to store the
changes to your assignment, and will be where you submit your solution.
Do not rename the homework files. Do not modify anything inside the
`.github/` folder. The only files that should need editing for this
assignment are `cse110A_parser.py` and `local_value_numbering.py`.  
  
To commit any changes in the main branch. This can be done via the Git
command line with the following:

```bash
git add cse110A_parser.py local_value_numbering.py  # Add required files.
git commit -m "write your message"                  # Commit with a message.
git push                                             # Push to GitHub.
```

Again, this can be accomplished with other Git tools as long as the
"main" branch contains the final submission of the Assignment.  
  
As a reminder, it is advisable not to wait until the last minute to
submit to Gradescope and GitHub Classroom, as all of the autograding is
run in a queue, and it may take some time to get results for your
submission.

[^1]: By parsing, I am not talking about a scanner or a top down parser.
    I simply mean using a regular expression in Python to match simple
    patterns
