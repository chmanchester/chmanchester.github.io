    Title: Defining Semi-Automatic Test Selection
    Date: 2015-07-23T10:50:50
    Tags: DRAFT

We run a lot of tests against Firefox, and running them takes a long time.
Ideally, a best effort test selection is done on try, and if nothing obvious
shakes out of that a change is landed on inbound. Inbound runs a selection of
test jobs pared down based on coarse historical data (SETA) and periodically
defaults to a full set of test jobs. Modulo a handful of periodic build types,
the remaining integration branches run a full set of tests and builds.

With the exception of the (optional) selection made by try syntax, none of
these test selection mechanisms codify the idea that given a particular change,
some tests might fail, and others are unlikely to fail or never will.

So, what does smarter test selection look like?

# Inputs
* Source file changes
* Mapping from source files to test files
* Mapping from source files to impacted platforms
* Mapping from test files to an idea of how to run them

# Outputs
* Platforms
* Build types
* Test suites
* Test sets

Here's a sketch of how this could work:

* Source file changes come from hg.
* The build system (particularly moz.build file metadata) establishes an
  initial mapping from source files to test files based on the proximity of
  sources to test manifests and user input (annotations).
* Auxilliary sources of data enhance and expand dependency information. A
  variety of sources could be useful here:
  * Static analysis: if A includes B, breaking B could break A
  * Code coverage: if A runs as a result of running test B, test B might
    fail because of a change to A. If A does not run as a result of test
    B, test B is unlikely to fail as a result of a change to A.
  * 


<!-- more -->
