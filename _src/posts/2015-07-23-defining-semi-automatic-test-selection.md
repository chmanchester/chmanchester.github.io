    Title: Defining Semi-Automatic Test Prioritization
    Date: 2015-07-23T10:50:50
    Tags: mozilla

We run a lot of tests against Firefox, and running them takes a long time.
Ideally, a best effort test selection is done on try, and if nothing obvious
shakes out of that a change is landed on inbound. Inbound runs a selection of
test jobs pared down based on coarse historical data (SETA) and periodically
defaults to a full set of test jobs. Modulo a handful of periodic build types,
the remaining integration branches run a full set of tests and builds.
<!-- more -->

With the exception of the (optional) selection made by try syntax, none of
these test selection mechanisms codify the idea that some tests might fail
given a particular change, and others are unlikely to fail or never will.

What could smarter test prioritization look like?

## Inputs
* Source file changes
* A set of test files given a set of changed source files
* A set of test platforms given a set of changed source files
* A mapping from test files to an idea of how to run them

## Outputs
* Platforms
* Build types
* Test suites
* Test sets

Here's a sketch of how this could work. This list doesn't have a lot of regard
for what I think is feasible in the short term. Note, in particular, that code
coverage data for Mozilla's code base is not readily available.

* Source file changes for a push come from hg. This should be extractable
  from a user's machine or hg.mozilla.org servers.
* The build system (particularly moz.build file metadata) establishes an
  initial mapping from source files to test files based on the proximity of
  sources to test manifests and user input (annotations).
* Auxilliary sources of data enhance and expand dependency information. A
  variety of sources could be useful:
  * Static dependencies: if A includes B, changing B could break A. This could
    be taken from static analysis of source files (actual includes), or
    estimated based on dependencies defined by the build system.
  * Code coverage: if A runs as a result of test B, test B might fail because
    of a change to A. If A does not run as a result of test B, test B is
    unlikely to fail as a result of a change to A.
    (Many test case prioritization techniques in the literature involve
     code coverage in some way[^fn])
  * Historical data: if changing A has made test B fail in the past, perhaps
    it will do so again in the future. If changing A very frequently causes
    test failures, perhaps we should expand our test set for a change to A.
* Dependency information maps changes in source files to test files, and
  the build system provides information on how to run this file as a test.
  It's the case in general that there's one way to run a test file, and
  we can determine by looking at the manifest that includes it how to run
  that file (this is the test "flavor" in Mozilla build system
  nomenclature). From this we have the test of tests and test suites to
  run, which could be fed into try syntax to select automation jobs or
  written to a set of manifests to drive running tests locally.

The remaining outputs to acquire after going through these steps are
platforms and build types. Build types are things like opt, debug, and pgo,
while platforms are things like Windows or B2G. In general we can't
expect a set of changed files to suggest a test platform and build type
(many changes will impact code that runs across platforms and build types),
but when all changes are in a subdirectory known to only impact Windows or
Android (for instance), we can be confident selecting only those platforms.
I expect these classifications to be coarse and stable enough to capture
effectively with annotations in moz.build files.

This is my general plan of attack for getting this off the ground in
the coming months (initial work tracked in [bug 1184405](https://bugzilla.mozilla.org/show_bug.cgi?id=1184405)).

Feedback is welcome and appreciated.

[^fn]: For instance, ["Prioritizing test cases for regression testing"](http://digitalcommons.unl.edu/cgi/viewcontent.cgi?article=1031&context=csetechreports)
