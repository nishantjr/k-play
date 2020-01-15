==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/README.md <==

## Part 1: Defining LAMBDA

Here you will learn how to define a very simple language in K and the basics
of how to use the K tool.  The language is a variant of call-by-value lambda
calculus and its definition is based on substitution.  Specifically, you will
learn the following:

* How to define a module.
* How to define a language syntax.
* How to use the defined syntax to parse programs.
* How to import predefined modules.
* How to define evaluation strategies using strictness attributes.
* How to define semantic rules.
* How the predefined generic substitution works.
* How to generate PDF and HTML documentation from ASCII definitions.
* How to include builtins (integers and Booleans) into your language.
* How to define derived language constructs.


==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_1/README.md <==

### Syntax Modules and Basic K Commands

[MOVIE [4'07"]](http://youtu.be/y5Tf1EZVj8E)

Here we define our first K module, which contains the initial syntax of the
LAMBDA language, and learn how to use the basic K commands.

Let us create an empty working folder, and open a terminal window
(to the left) and an editor window (to the right).  We will edit our K
definition in the right window in a file called `lambda.k`, and will call
the K tool commands in the left window.

Let us start by defining a K module, containing the syntax of LAMBDA.

K modules are introduced with the keywords `module` ... `endmodule`.

The keyword `syntax` adds new productions to the syntax grammar, using a
BNF-like notation.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_2/README.md <==

### Module Importing, Rules, Variables

[MOVIE [4'03"]](http://youtu.be/NDXgYfHG6R4)

We here learn how to include a predefined module (SUBSTITUTION), how to
use it to define a K rule (the characteristic rule of lambda calculus),
and how to make proper use of variables in rules.

Let us continue our `lambda.k` definition started in the previous lesson.

The `requires` keyword takes a `.k` file containing language features that
are needed for the current definition, which can be found in the
[k/include](/include/) folder.  Thus, the command

    require "substitution.k"

says that the subsequent definition of LAMBDA needs the generic substitution,
which is predefined in file `substitution.k` under the folder

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_3/README.md <==

### Evaluation Strategies using Strictness

[MOVIE [2'20"]](http://youtu.be/aul1x6bd1YM)

Here we learn how to use the K `strict` attribute to define desired evaluation
strategies.  We will also learn how to tell K which terms are already
evaluated, so it does not attempt to evaluate them anymore and treats them
internally as results of computations.

Recall from the previous lecture that the LAMBDA program
`free-variable-capture.lambda` was stuck, because K was not given permission
to evaluate the arguments of the lambda application construct.

You can use the attribute `strict` to tell K that the corresponding construct
has a strict evaluation strategy, that is, that its arguments need to be
evaluated before the semantics of the construct applies.  The order of
argument evaluation is purposely unspecified when using `strict`, and indeed
the K tool allows us to detect all possible non-deterministic behaviors that

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_4/README.md <==

### Generating Documentation; Latex Attributes

[MOVIE [3'13"]](http://youtu.be/ULXA4e_6-DY)

In this lesson we learn how to generate formatted documentation from K
language definitions.  We also learn how to use Latex attributes to control
the formatting of language constructs, particularly of ones which have a
mathematical flavor and we want to display accordingly.

To generate PDF documentation, all we have to do is to call the `kdoc` command
in the folder where the kompiled definition is.  This command generates a
`lambda.pdf` file, which contains the formatted K definition.

Open this file using your favorite PDF reader.  The syntactic details are not
shown in the generated PDF, because we typically want to focus on semantics at
this stage.  The main notational difference between the original `.k` and the
generated PDF is how rules are displayed.  In the PDF, the rule `left => right`
is replaced by its representation in K (see papers on K), which is harder to

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_5/README.md <==

### Adding Builtins; Side Conditions

[MOVIE [4'52"]](http://youtu.be/T1aI04q3l9U)

We have already added the builtin identifiers (sort `Id`) to LAMBDA expressions,
but those had no operations on them.  In this lesson we add integers and
Booleans to LAMBDA, and extend the builtin operations on them into
corresponding operations on LAMBDA expressions.  We will also learn how to add
side conditions to rules, to limit the number of instances where they can
apply.

The K tool provides several builtins, which are automatically included in all
definitions.  These can be used in the languages that we define, typically by
including them in the desired syntactic categories.  You can also define your
own builtins in case the provided ones are not suitable for your language
(e.g., the provided builtin integers and operations on them are arbitrary
precision).


==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_6/README.md <==

### Selective Strictness; Anonymous Variables

[MOVIE [2'14"]](http://youtu.be/IreP6DFPWdk)

We here show how to define selective strictness of language constructs,
that is, how to state that certain language constructs are strict only
in some arguments.  We also show how to use anonymous variables.

We next define a conditional `if` construct, which takes three arguments,
evaluates only the first one, and then reduces to either the second or the
third, depending on whether the first one evaluated to true or to false.

K allows to define selective strictness using the same `strict` attribute,
but passing it a list of numbers.  The numbers correspond to the arguments
in which we want the defined construct to be strict.  In our case,

    syntax Exp ::= "if" Exp "then" Exp "else" Exp   [strict(1)]


==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_7/README.md <==

### Derived Constructs, Extending Predefined Syntax

[MOVIE [5'10"]](http://youtu.be/qZWiBaN7zrw)

In this lesson we will learn how to define derived language constructs, that
is, ones whose semantics is defined completely in terms of other language
constructs.  We will also learn how to add new constructs to predefined
syntactic categories.

When defining a language, we often want certain language constructs to be
defined in terms of other constructs.  For example, a let-binding construct
of the form

    let x = e in e'

is nothing but syntactic sugar for

    (lambda x . e') e

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_8/README.md <==

### Multiple Binding Constructs

[MOVIE [2'40"]](http://youtu.be/Ox4uXDpcY64)

Here we learn how multiple language constructs that bind variables can
coexist. We will also learn about or recall another famous binder besides
`lambda`, namely `mu`, which can be used to elegantly define all kinds of
interesting fixed-point constructs.

The `mu` binder has the same syntax as lambda, except that it replaces
`lambda` with `mu`.

Since `mu` is a binder, in order for substitution to know how to deal with
variable capture in the presence of `mu`, we have to tell it that `mu` is a
binding construct, same like lambda.  We take advantage of being there and
also add `mu` its desired latex attribute.

The intuition for

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/1_lambda/lesson_9/README.md <==

### A Complete and Documented K Definition

[MOVIE [6'07"]](http://youtu.be/-pHgLqNMKac)

In this lesson you will learn how to add formal comments to your K definition,
in order to nicely document it.  The generated document can be then used for
various purposes: to ease understanding the K definition, to publish it,
to send it to others, etc.

The K tool allows a literate programming style, where the executable
language definition can be documented by means of annotations.  Some
back-ends of the K tool, such as those enabled with the `--latex`, `--pdf` and
the `--html` options to the `kompile` tool, know how to interpret such
annotations.

There are three types of comments, which we discuss next.

#### Ordinary comments

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/2_imp/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

## Part 2: Defining IMP

Here you will learn how to define a very simple imperative language in K
and the basics of how to work with configurations, cells, and computations.
Specifically, you will learn the following:

* How to define languages using multiple modules.
* How to define sequentially strict syntactic constructs.
* How to use K's syntactic lists.
* How to define, initialize and configure configurations.
* How the language syntax is swallowed by the builtin K syntactic category.
* The additional syntax of the K syntactic category.
* How the strictness annotations are automatically desugared into rules.
* The first steps of the configuration abstraction mechanism.
* The distinction between structural and computational rules.

Like in the previous tutorial, this folder contains several lessons, each
adding new features to IMP.  Do them in order.  Also, make sure you completed

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/2_imp/lesson_1/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Defining a More Complex Syntax

Here we learn how to define a more complex language syntax than LAMBDA's,
namely the C-like syntax of IMP.  Also, we will learn how to define languages
using multiple modules, because we are going to separate IMP's syntax from
its semantics using modules.  Finally, we will also learn how to use K's
builtin support for syntactic lists.

The K tool provides modules for grouping language features.  In general, we
can organize our languages in arbitrarily complex module structures.
While there are no rigid requirements or even guidelines for how to group
language features in modules, we often separate the language syntax from the
language semantics in different modules.

In our case here, we start by defining two modules, IMP-SYNTAX and IMP, and
import the first in the second, using the keyword `imports`.  As their names
suggest, we will place all IMP's syntax definition in IMP-SYNTAX and all its
semantics in IMP.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/2_imp/lesson_2/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Defining a Configuration

Here we learn how to define a configuration in K.  We also learn how to
initialize and how to display it.

As explained in the overview presentation on K, configurations are quite
important, because all semantic rules match and apply on them.
Moreover, they are the backbone of *configuration abstraction*, which allows
you to only mention the relevant cells in each semantic rule, the rest of
the configuration context being inferred automatically.  The importance of
configuration abstraction will become clear when we define more complex
languages (even in IMP++).  IMP does not really need it.  K configurations
are constructed making use of cells, which are labeled and can be arbitrarily
nested.

Configurations are defined with the keyword `configuration`.  Cells are
defined using an XML-ish notation stating clearly where the cell starts
and where it ends.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/2_imp/lesson_3/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Computations, Results, Strictness; Rules Involving Cells

In this lesson we will learn about the syntactic category `K` of computations,
about how strictness attributes are in fact syntactic sugar for rewrite rules
over computations, and why it is important to tell the tool which
computations are results.  We will also see a K rule that involves cells.

##### K Computations

Computation structures, or more simply *computations*, extend the abstract
syntax of your language with a list structure using `~>` (read  *followed
by* or *and then*, and written $\curvearrowright$ in Latex) as a separator.
K provides a distinguished sort, `K`, for computations.  The extension of the
abstract syntax of your language into computations is done automatically by
the K tool when you declare constructs using the `syntax` keyword, so the K
semantic rules can uniformly operate only on terms of sort `K`.  The intuition
for computation structures of the form


==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/2_imp/lesson_4/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Configuration Abstraction, Part 1; Types of Rules

Here we will complete the K definition of IMP and, while doing so, we will
learn the very first step of what we call *configuration abstraction*, and
the semantic distinction between structural and computational rules.

#### The IMP Semantic Rules

Let us add the remaining rules, in the order in which the language constructs
were defined in IMP-SYNTAX.

The rules for the arithmetic and Boolean constructs are self-explanatory.
Note, however, that K will infer the correct sorts of all the variables in
these rules, because they appear as arguments of the builtin operations
(`_+Int_`, etc.).  Moreover, the inferred sorts will be enforced dynamically.
Indeed, we do not want to apply the rule for addition, for example, when the
two arguments are not integers.  In the rules for `&&`, although we prefer to
not do it here for simplicity, we could have eliminated the dynamic check by

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/2_imp/lesson_5/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Completing and Documenting IMP

We here learn no new concepts, but it is a good moment to take a break
and contemplate what we learned so far.

Let us add lots of formal annotations to `imp.k`.

Once we are done with the annotations, we kompile with the documentation
option and then take a look at the produced document.  We often call these
documents *language posters*.  Depending on how much information you add to
these language posters, they can serve as standalone, formal presentations
of your languages.  For example, you can print them as large posters and
post them on the wall, or in poster sessions at conferences.

This completes our second tutorial.  The next tutorials will teach us more
features of the K framework, such as how to define languages with complex
control constructs (like `callcc`), languages which are concurrent, and so on.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/3_lambda++/README.md <==

## Part 3: Defining LAMBDA++

Here you will learn how to define language constructs which abruptly change
the execution control flow, and how to define language semantics following
and environment/store style.  Specifically, you will learn the following:

* How to define constructs like `callcc`, which allow you to take snapshots of
  program executions and to go *back in time* at any moment.
* How to define languages in an environment/store style.
* Some basic notions about the use of closures and closure-like semantic
  structures to save and restore execution environments.
* Some basic intuitions about reusing existing semantics in new languages,
  as well as some of the pitfalls in doing so.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/3_lambda++/lesson_1/README.md <==

### Abrupt Changes of Control

[MOVIE [6'28"]](http://youtu.be/UZ9iaus024g)

Here we add *call-with-current-continuation* (`callcc`) to the definition of
LAMBDA completed in Tutorial 1, and call the resulting language LAMBDA++.
While doing so, we will learn how to define language constructs that
abruptly change the execution control flow.

Take over the `lambda.k` definition from Lesson 8 in Part 1 of this Tutorial,
which is the complete definition of the LAMBDA language, but without the
comments.

`callcc` is a good example for studying the capabilities of a framework to
support abrupt changes of control, because it is one of the most
control-intensive language constructs known.  Scheme is probably the first
programming language that incorporated the `callcc` construct, although
similar constructs have been recently included in many other languages in

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/3_lambda++/lesson_2/README.md <==

### Semantic (Non-Syntactic) Computation Items

[MOVIE [8'02"]](http://youtu.be/BYhQQW6swfc)

In this lesson we start another semantic definition of LAMBDA++, which
follows a style based on environments instead of substitution.  In terms of
K, we will learn how easy it is to add new items to the syntactic category
of computations `K`, even ones which do not have a syntactic nature.

An environment binds variable names of interest to locations where their
values are stored.  The idea of environment-based definitions is to maintain
a global *store* mapping locations to values, and then have environments
available when we evaluate expressions telling where the variables are
located in the store.  Since LAMBDA++ is a relatively simple language, we
only need to maintain one global environment.  Following a similar style
like in IMP, we place all cells into a top cell `T`:

    configuration <T>

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/3_lambda++/lesson_3/README.md <==
<!-- Copyright (c) 2014-2019 K Team. All Rights Reserved. -->

### Reusing Existing Semantics

[MOVIE [3'21"]](http://youtu.be/tW4KRdgBIGo)

In this lesson we will learn that, in some cases, we can reuse existing
semantics of language features without having to make any change!

Although the definitional style of the basic LAMBDA language changed quite
radically in our previous lesson, compared to its original definition in
Part 1 of the tutorial, we fortunately can reuse a large portion of the
previous definition.  For example, let us just cut-and-paste the rest of the
definition from Lesson 7 in Part 1 of the tutorial.

Let us `kompile` and `krun` all the remaining programs from Part 1 of the
tutorial.  Everything should work fine, although the store contains lots of
garbage.  Garbage collection is an interesting topic, but we do not do it
here.  Nevertheless, much of this garbage is caused by the intricate use of
the fixed-point combinator to define recursion.  In a future lesson in this

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/3_lambda++/lesson_4/README.md <==
<!-- Copyright (c) 2014-2019 K Team. All Rights Reserved. -->

### Do Not Reuse Blindly!

[MOVIE [3'37"]](http://youtu.be/OXvtklaSaSQ)

It may be tempting to base your decision to reuse an existing semantics of
a language feature solely on syntactic considerations; for example, to reuse
whenever the parser does not complain.  As seen in this lesson, this could
be quite risky.

Let's try (and fail) to reuse the definition of `callcc` from Lesson 1:

    syntax Exp ::= "callcc" Exp  [strict]
    syntax Val ::= cc(K)
    rule <k> (callcc V:Val => V cc(K)) ~> K </k>
    rule <k> cc(K) V ~> _ =>  V ~> K </k>

The `callcc` examples that we tried in Lesson 1 work, so it may look it works.


==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/3_lambda++/lesson_5/README.md <==
<!-- Copyright (c) 2014-2019 K Team. All Rights Reserved. -->

### More Semantic Computation Items

[MOVIE [5'19"]](http://youtu.be/dP3FW0kZN6k)

In this lesson we see more examples of semantic (i.e., non-syntactic)
computational items, and how useful they can be.  Specifically, we fix the
environment-based definition of `callcc` and give an environment-based
definition of the `mu` construct for recursion.

Let us first fix `callcc`.  As discussed in Lesson 4, the problem that we
noticed there was that we only recovered the computation, but not the
environment, when a value was passed to the current continuation.  This is
quite easy to fix: we modify `cc` to take both an environment and a
computation, and its rules to take a snapshot of the current environment with
it, and to recover it at invocation time:

    syntax Val ::= cc(Map,K)
    rule <k> (callcc V:Val => V cc(Rho,K)) ~> K </k> <env> Rho </env>

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/3_lambda++/lesson_6/README.md <==

### Wrapping Up and Documenting LAMBDA++

[MOVIE [6'23"]](http://youtu.be/xfvx6Ss5PcA)

In this lesson we wrap up and nicely document LAMBDA++.  In doing so, we also
take the freedom to reorganize the semantics a bit, to make it look better.

See the `lambda.k` file, which is self-explanatory.

Part 3 of the tutorial is now complete.  Part 4 will teach you more features
of the K framework, in particular how to exhaustively explore the behaviors
of non-deterministic or concurrent programs.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

## Part 4: Defining IMP++

IMP++ extends IMP, which was discussed in Part 2 of this tutorial, with several
new syntactic constructs.  Also, some existing syntax is generalized, which
requires non-modular changes of the existing IMP semantics.  For example,
global variable declarations become local declarations and can occur
anywhere a statement can occur.  In this tutorial we will learn the following:

* That (and how) existing syntax/semantics may change as a language evolves.
* How to refine configurations as a language evolves.
* How to define and use fresh elements of desired sorts.
* How to tag syntactic constructs and rules, and how to use such tags
  with the `superheat`/`supercool`/`transition` options of `kompile`.
* How the `search` option of `krun` works.
* How to stream cells holding semantic lists to the standard input/output,
  and thus obtain interactive interpreters for the defined languages.
* How to delete, save and restore cell contents.
* How to add/delete cells dynamically.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_1/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Extending/Changing an Existing Language Syntax

Here we learn how to extend the syntax of an existing language, both with
new syntactic constructs and with more general uses of existing constructs.
The latter, in particular, requires changes of the existing semantics.

Consider the IMP language, as defined in Lesson 4 of Part 2 of the tutorial.

Let us first add the new syntactic constructs, with their precedences:

- variable increment, `++`, which increments an integer variable and
evaluates to the new value;
- `read`, which reads and evaluates to a new integer from the input buffer;
- `print`, which takes a comma-separated list of arithmetic expressions and
  evaluates and prints each of them in order, from left to right, to the
  output buffer; we therefore define a new list syntactic category, `AExps`,
  which we pass as an argument to `print`; note we do not want to declare
  `print` to be `strict`, because we do not want to first evaluate the

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_2/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Configuration Refinement; Freshness

To prepare for the semantics of threads and local variables, in this lesson we
split the state cell into an environment and a store.  The environment and
the store will be similar to those in the definition of LAMBDA++ in Part
3 of the Tutorial.  This configuration refinement will require us to change
some of IMP's rules, namely those that used the state.

To split the state map, which binds program variables to values, into an
environment mapping program variables to locations and a store mapping
locations to values, we replace in the configuration declaration the cell

    <state color="red"> .Map </state>

with two cells

    <env color="LightSkyBlue"> .Map </env>
    <store color="red"> .Map </store>

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_3/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Tagging; Transition Kompilation Option

In this lesson we add the semantics of variable increment.  In doing so, we
learn how to tag syntactic constructs and rules and then use such tags to
instruct the `kompile` tool to generate the desired language model that is
amenable for exhaustive analysis.

The variable increment rule is self-explanatory:

    rule <k> ++X => I +Int 1 ...</k>
         <env>... X |-> N ...</env>
         <store>... N |-> (I => I +Int 1) ...</store>

We can now run programs like our `div.imp` program introduced in Lesson 1.
Do it.

The addition of increment makes the evaluation of expressions have side
effects.  That, in combination with the non-determinism allowed by the

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_4/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Semantic Lists; Input/Output Streaming

In this lesson we add semantics to the `read` and `print` IMP++ constructs.
In doing so, we also learn how to use semantic lists and how to connect
cells holding semantic lists to the standard input and standard output.
This allows us to turn the K semantics into an interactive interpreter.

We start by adding two new cells to the configuration,

    <in color="magenta"> .List </in>
    <out color="Orchid"> .List </out>

each holding a semantic list, initially empty.  Semantic lists are
space-separated sequences of items, each item being a term of the form
`ListItem(t)`, where `t` is a term of sort `K`.  Recall that the semantic maps,
which we use for states, environments, stores, etc., are sets of pairs
`t1 |-> t2`, where `t1` and `t2` are terms of sort K.  The `ListItem` wrapper
is currently needed, to avoid parsing ambiguities.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_5/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Deleting, Saving and Restoring Cell Contents

In this lesson we will see how easily we can delete, save and/or restore
contents of cells in order to achieve the desired semantics of language
constructs that involve abrupt changes of control or environments.  We have
seen similar or related K features in the LAMBDA++ language in Part 3 of the
tutorial.

Let us start by adding semantics to the `halt` statement.  As its name says,
what we want is to abruptly terminate the execution of the program.  Moreover,
we want the program configuration to look as if the program terminated
normally, with an empty computation cell.  The simplest way to achieve that is
to simply empty the computation cell when `halt` is encountered:

    rule <k> halt; ~> _ => . </k>

It is important to mention the entire `<k/>` cell here, with both its membranes
closed, to make sure that its entire contents is discarded.  Note the

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_6/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Adding/Deleting Cells Dynamically; Configuration Abstraction, Part 2

In this lesson we add dynamic thread creation and termination to IMP, and
while doing so we learn how to define and use configurations whose structure
can evolve dynamically.

Recall that the intended semantics of `spawn S` is to spawn a new concurrent
thread that executes `S`.  The new thread is being passed at creation time
its parent's environment, so it can share with its parent the memory
locations that its parent had access to at creation time.  No other locations
can be shared, and no other memory sharing mechanism is available.
The parent and the child threads can evolve unrestricted, in particular they
can change their environments by declaring new variables or shadowing existing
ones, can create other threads, and so on.

The above suggests that each thread should have its own computation and its
own environment.  This can be elegantly achieved if we group the `<k/>` and
`<env/>` cells in a `<thread/>` cell in the configuration.  Since at any given

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_7/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Everything Changes: Syntax, Configuration, Semantics

In this lesson we add thread joining, one of the simplest thread
synchronization mechanisms.  In doing so, we need to add unique ids
to threads in the configuration, and to modify the syntax to allow `spawn`
to return the id of the newly created thread.  This gives us an opportunity
to make several other small syntactic and semantics changes to the language,
which make it more powerful or more compact at a rather low cost.

Before we start, let us first copy and modify the previous `spawn.imp` program
from Lesson 1 to make use of thread joining.  Recall from Lesson 6 that in some
runs of this program the main thread completed before the child threads,
printing a possibly undesired value of `x`.  What we want now is to assign
unique ids to the two spawned threads, and then to modify the main thread to
join the two child threads before printing.  To avoid adding a new type to
the language, let's assume that thread ids are integer numbers.  So we declare
two integers, `t1` and `t2`, and assign them the two spawn commands.  In order
for this to parse, we will have to change the syntax of `spawn` to be an

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/4_imp++/lesson_8/README.md <==
<!-- Copyright (c) 2010-2019 K Team. All Rights Reserved. -->

### Wrapping up Larger Languages

In this lesson we wrap up IMP++'s semantics and also generate its poster.
While doing so, we also learn how to display larger configurations in order
to make them easier to read and print.

Note that we rearrange a bit the semantics, to group the semantics of old
IMP's constructs together, and separate it from the new IMP++'s semantics.

Once we are done adding all the comments, we `kompile` with the documentation
option `--pdf`, and then visualize the generated PDF document.

Everything looks good, except the configuration.  We can move the store, in
and out cells on the next line.  The html-ish tag `<br/>` inserts a line break
in the configuration layout, as you can see in the generated document.

There is a detailed discussion at the end of the document about the
`--transition` option of `kompile`, because that is important and we want

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/README.md <==

## Part 5: Defining Type Systems

In this part of the tutorial we will show that defining type systems for
languages is essentially no different from defining semantics.  The major
difference is that programs and fragments of programs now rewrite to their
types, instead of to concrete values.  In terms of K, we will learn how
to use it for a certain particular but important kind of applications.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_1/README.md <==
<!-- Copyright (c) 2014-2019 K Team. All Rights Reserved. -->

### Imperative, Environment-Based Type Systems

[MOVIE [10'11"]](http://youtu.be/WyUxdo7GhtE)

In this lesson you learn how to define a type system for an imperative
language (the IMP++ language defined in Part 4 of the tutorial), using a style
based on type  environments.

Let us copy the `imp.k` file from Part 4 of the tutorial, Lesson 7, which holds
the semantics of IMP++, and modify it into a type system.  The resulting type
system, when executed, yields a type checker.

We start by defining the new strictness attributes of the IMP++ syntax.
While doing so, remember that programs and fragments of programs now reduce
to their types.  So types will be the new results of our new (type) semantics.
We also clean up the semantics by removing the unnecessary tags, and also
use `strict` instead of `seqstrict` wherever possible, because `strict` gives
implementations more freedom.  Interestingly, note that `spawn` is strict now,

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_2/README.md <==
<!-- Copyright (c) 2014-2019 K Team. All Rights Reserved. -->

### Substitution-Based Higher-Order Type Systems

[MOVIE [6'52"]](http://youtu.be/7P2QtR9jM2o)

In this lesson you learn how to define a substitution-based type system for
a higher-order language, namely the LAMBDA language defined in Part 1 of the
tutorial.

Let us copy the definition of LAMBDA from Part 1 of the tutorial, Lesson 8.
We are going to modify it into a type systems for LAMBDA.

Before we start, it is important to clarify an important detail, namely that
our type system will yield a type checker when executed, not a type
inferencer.  In particular, we are going to change the LAMBDA syntax
to allow us to associate a type to each declared variable.  The
constructs which declare variables are `lambda`, `let`, `letrec` and `mu`.
The syntax of all these will therefore change.


==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_3/README.md <==

### Environment-Based Higher-Order Type Systems

[MOVIE []]()

In this lesson you learn how to define an environment-based type system for
a higher-order language, namely the LAMBDA language defined in Part 1 of the
tutorial.

The simplest and fastest way to proceed is to copy the substitution-based
type system of LAMBDA from the previous lesson and modify it into an
environment-based one.  A large portion of the substitution-based definition
will remain unchanged.  We only have to modify the rules that use
substitution.

We do not need the substitution anymore, so we can remove the require and
import statements.  The syntax of types and expressions stays unchanged, but
we can now remove the `binder` tag of lambda.


==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_4/README.md <==
<!-- Copyright (c) 2014-2019 K Team. All Rights Reserved. -->

### A Naive Substitution-Based Type Inferencer

[MOVIE []]()

In this lesson you learn how to define a naive substitution-based type
inferencer for a higher-order language, namely the LAMBDA language
defined in Part 1 of the tutorial.

Unlike in the type checker defined in Lessons 2 and 3, where we had to
associate a type with each declared variable, a type inferencer
attempts to infer the types of all the variables from the way those
variables are used.  Let us take a look at this program, say `plus.lambda`:

    lambda x . lambda y . x + y

Since `x` and `y` are used in an integer addition context, we can infer
that they must have the type `int` and the result of the addition is
also an `int`, so the type of the entire expression is `int -> int -> int`.

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_5/README.md <==

### A Naive Environment-Based Type Inferencer

[MOVIE []]()

In this lesson you learn how to define a naive environment-based type
inferencer for a higher-order language.  Specifically, we take the
substitution-based type inferencer for LAMBDA defined in Lesson 4 and
turn it into an environment-based one.

Recall from Lesson 3, where we defined an environment-based type
checker for LAMBDA based on the substitution-based one in Lesson 2,
that the transition from a substitution-based definition to an
environment-based one was quite systematic and mechanical: each
substitution occurrence `E[T/X]` is replaced by `E`, but at the same time
the variable `X` is bound to type `T` in the type environment.  One benefit
of using type environments instead of substitution is that we replace
a linear complexity operation (the substitution) with a constant
complexity one (the variable lookup).

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_6/README.md <==

### Parallel Type Checkers/Inferencers

[MOVIE []]()

In this lesson you learn how to define parallel type checkers or
inferencers.  For the sake of a choice, we will parallelize the one in
the previous lesson, but the ideas are general.  We are using the same
idea to define type checkers for other languages in the K tool
distribution, such as SIMPLE and KOOL.

The idea is in fact quite simple.  Instead of one monolithic typing
task, we generate many smaller tasks, which can be processed in
parallel.  We use the same approach to define parallel semantics as we
used for threads in IMP++ in Part 4 of the tutorial, that is, we add a
cell holding all the parallel tasks, making sure we declare the cell
holding a task with multiplicity `*`.  For the particular type
inferencer that we chose here, the one in Lesson 5, each task will
hold an expression to type together with a type environment (so it

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_7/README.md <==

### A Naive Substitution-based Polymorphic Type Inferencer

[MOVIE []]()

In this lesson you learn how little it takes to turn a naive monomorphic
type inferencer into a naive polymorphic one, basically only changing
a few characters.  In terms of the K framework, you will learn that
you can have complex combinations of substitutions in K, both over
expressions and over types.

Let us start directly with the change.  All we have to do is to take
the LAMBDA type inferencer in Lesson 4 and only change the macro

    rule let X = E in E' => (lambda X . E') E  [macro]

as follows:

    rule let X = E in E' => E'[E/X]  [macro]

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_8/README.md <==

### A Naive Environment-based Polymorphic Type Inferencer

[MOVIE []]()

In this short lesson we discuss how to quickly turn a naive
environment-based monomorphic type inferencer into a naive let-polymorphic
one.  Like in the previous lesson, we only need to change a few
characters.  In terms of the K framework, you will learn how to have
both environments and substitution in the same definition.

Like in the previous lesson, all we have to do is to take the LAMBDA
type inferencer in Lesson 5 and only change the macro

    rule let X = E in E' => (lambda X . E') E  [macro]

as follows:

    rule let X = E in E' => E'[E/X]  [macro]

==> /home/njr/co/github.com/kframework/k5/k-distribution/tutorial/1_k/5_types/lesson_9/README.md <==
<!-- Copyright (c) 2014-2019 K Team. All Rights Reserved. -->

### Let-Polymorphic Type Inferencer (Damas-Hindley-Milner)

[MOVIE []]()

In this lesson we discuss a type inferencer based on what we call today
*the Damas-Hindley-Milner type system*, which is at the core of many
modern functional programming languages.  The first variant of it was
proposed by Hindley in 1969, then, interestingly, Milner rediscovered
it in 1978 in the context of the ML language.  Damas formalized it as
a type system in his PhD thesis in 1985.  More specifically, our type
inferencer here, like many others as well as many implementations of
it, follows more closely the syntax-driven variant proposed by Clement
in 1987.

In terms of K, we will see how easily we can turn one definition which
is considered naive (our previous type inferencer in Lesson 8) into a
definition which is considered advanced.  All we have to do is to
change one existing rule (the rule of the let binder) and to add a new
