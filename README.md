This repository begins to document various features and commonly used design 
patterns in K.

Dependencies
============

* `pandoc` (on ubuntu `sudo apt install pandoc`).
* `python3`
* `pyyaml` (on ubuntu `sudo apt install python3-pip` and then `pip3 install pyyaml`)

Running tests
=============

* To build a definition, run `./build <definition>`
* To build all tests for a definition, run `./build <definition>-tests`
* To build tests for a definition against one backend, run `./build <definition>-<backend>-tests`
