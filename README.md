This repository begins to document various features and commonly used design 
patterns in K.

Dependencies
============

* `pandoc` (on ubuntu `sudo apt install pandoc`).
* `python3`
* `pyyaml` (on ubuntu `sudo apt install python3-pip` and then `pip3 install pyyaml`)

Running tests
=============

Note that all tests do not work on all backends, and that some backends fail
only because they produce non-standard output.

TODO: Add a list of tests/backend pairs that are known to be failing

* To build a definition, run `./build <definition>`
* To build all tests for a definition, run `./build <definition>-tests`
* To build tests for a definition against one backend, run `./build <definition>-<backend>-tests`

Organization
============

[pending-documentation.md] contains documentation and TODOs that need examples
and tests. Each other markdown document contains a K definition (in code blocks
with attribute `k`) as well as examples/tests (in code blocks with attribute
containing `.input`) and expected output (attribute `.expected`). The attribute
for each pair of test/expected output also has the name for the test (e.g.
`.my-test`). This identifier must be listed in the `tests` array in the YAML
frontmatter at the beginning of the file.

