Build processs and stucture of a kompiled definition
----------------------------------------------------

`kompile` outputs everything it needs to a directory with the suffix `*-kompiled`.
This is placed in the directory specified by `--directory` (defaulting to the
current working directory).
Depending on the backend you pick, this will be different.

In the case of
haskell/llvm backends the two important generated files are:

* `compiled.txt`: the kompiled K definition, things like
configuration composition, comfiguration completion, macro expansion, and other
kompile passes have been done, and
* `definition.kore`: which translated the
  `kompile`d K definition to Kore, the IR that LLVM/Haskell backend uses.
  The Haskell backend reads this file each time it needs to run a program.
  The LLVM backend uses it to build an executable `interpreter` also placed in this directory.

You rarely need to ever inspect these files. However, they are useful
for debugging when K does unexpected things (such as a rule not firing).

Most of the "modern" or "recent" semantics we have use Markdown sources for
everything. So the build process looks like

1. use Pandoc to convert Markdown -\> K in the .build/defn directory,
2. call `kompile` on K sources in the .build/defn directory. The `*-kompiled` directory will live in there.

For example, with KWasm, you have `.build/defn/llvm/test-kompiled` (because `test.k` is the "main" file), and `.build/defn/haskell/test-kompiled`.

Right now, it's up to projects individual build systems to manually separate the
`*-kompiled` directories for each backend. Hence we have
.build/defn/{llvm,haskell}/test-kompiled instead of
.build/defn/test-kompiled/{llvm,haskell}. This is arguably not great, but it's
how it is.

Module imports in K are not as well-behaved as those in Maude. In Maude, you can
say `protecting ...` , `including ...`, or `extending ...` depending on whether you
will:

(a) not modify anything defined in the definition,
(b) add new syntax to a given sort but leave things the same, or
(c) add new rules making more things equal.

In K everything is `including ...`, and there is not plans to change this,
because K modules are inherently non-modular.

Because of this, when you invoke `kprove` (for example, if you said
`kprove --directory .build/defn/haskell tests/specs/sum-to-n-spec.k --def-module VERIFICATION`),
it will take module `SUM-TO-N-SPEC` as the module with proof goals
to be discharged, and will take `VERIFICATION` and everything it includes as
axioms to use in discharging the proof. Proving *must* call kompile on the
`VERIFICATION` module, because it could be that even though `WASM-TEST` is your
"main module", `VERIFICATION` makes more things equal than `WASM-TEST` does, and so
the kompilation of `WASM-TEST` cannot be re-used. `kprove` does this automatically,
a large part of the start-up cost of calling kprove is invoking the kompile
pipeline.

