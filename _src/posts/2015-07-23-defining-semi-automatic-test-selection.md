    Title: Defining Semi-Automatic Test Prioritization
    Date: 2015-08-06T10:50:50
    Tags: Mozilla

We run a lot of tests against Firefox, and running them takes a long time.
Ideally, a best effort test selection is done on try, and if nothing obvious
shakes out of that a change is landed on inbound. Inbound runs a selection of
test jobs pared down based on coarse historical data (SETA). Modulo a handful
of periodic build types, the remaining integration branches run a full set of
tests and builds.

With the exception of the (entirely optional) selection made by try syntax,
none of these steps codify the idea that some tests might fail given a
particular change, and others are unlikely to fail or never will.

This post outlines one approach to implementing smarter (semi-automatic)
test selection for Gecko.

<!-- more -->

### Inputs
* Source file changes
* A set of test files given a set of changed source files
* A set of test platforms given a set of changed source files
* A mapping from test files to an idea of how to run them

### Outputs
* Platforms
* Build types
* Test sets
* Test suites

Here's a sketch of how this could work. This list doesn't have a lot of regard
for what I think is feasible in the short term. In particular, code coverage
data for Mozilla's code base is not readily available.

  * Source file changes for a push come from hg. This should be extractable
    from a user's machine or hg.mozilla.org.
  * The build system (particularly moz.build file metadata) establishes an
    initial mapping from source files to test files based on the proximity of
    sources to test manifests and user input (annotations).
  * Auxilliary sources of data enhance and expand dependency information. A
    variety of sources could be useful:
    * Static dependencies: if A includes B, changing B could break A. This could
      be derived from static analysis of source files (import/#includes), or
      estimated based on dependencies defined by the build system.
    * Code coverage: if A runs as a result of test B, test B might fail because
      of a change to A. If A does not run as a result of test B, test B is
      unlikely to fail as a result of a change to A.
      (Many test case prioritization techniques involve code coverage in some
      way[^fn])
    * Historical data: if changing A has made test B fail in the past, perhaps
      it will do so again in the future. If changing A very frequently causes
      test failures, perhaps we should expand our test set for a change to A.
  * The result is used to map source files to test files. Test files can
    usually be mapped to automation jobs by consulting the build system for
    a test flavor and finding the equivalent builder and suite category in
    automation.

After this process, platforms and build types are still not known. Build
types are things like opt, debug, and pgo, while platforms are things like
Windows or B2G. In general we can't expect a set of changed files to suggest
a test platform and build type (many changes will impact code that runs across
platforms and build types), but when all changes are in a subdirectory known
to impact a single platform, we can be confident selecting only that platform.
I expect these classifications are coarse and stable enough to capture
effectively with annotations in moz.build files.

This is the framework I'm planning to use to get this off the ground in the
coming months. Initial work around fleshing out moz.build file metadata is
tracked in [bug 1184405](https://bugzilla.mozilla.org/show_bug.cgi?id=1184405)).

Feedback is welcome and appreciated.

[^fn]: For instance, ["Prioritizing test cases for regression testing"](http://digitalcommons.unl.edu/cgi/viewcontent.cgi?article=1031&context=csetechreports)
