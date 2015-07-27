    Title: Introducing mach try
    Date: 2015-07-26T18:41:38
    Tags: mozilla

This is a short introduction to mach try, a mach command that simplifies the
process of selecting tests and pushing them to the try server.
<!-- more -->

To follow along at home you'll either need to be using git cinnabar or have
a modern mercurial and the hg "push-to-try" extension (available from |mach
mercurial-setup|). Append --no-push to commands to keep them from pushing
to the try server, and -n to see verbose messages associated with the results
of commands.

    # mach try is a command that takes try syntax and automates the steps
    # to push the current tree to try with that syntax.
    # For instance:

    $ ./mach try -p win32 -u mochitest-bc

    # ... will result in pushing "try: -b do -p win32 -u mochitest-bc -t none"
    # to try. This saves dealing with mq or other ways of generating the try
    # message commit. (An in-memory commit is generated with the appropriate
    # message -- mq is not invoked at any stage).

    # The more novel feature exposed by mach try is the ability to select
    # specific test directories containing xpcshell, mochitests or reftests
    # to run on the try server.

    # For instance, if I've just made a change to one of the python libraries
    # used by our test harnesses, and I'd like to quickly check that this
    # feature works on windows. I can run:

    $ ./mach try -p win64 testing/xpcshell testing/mochitest/tests

    # This will result in the small number of xpcshell and mochitest tests
    # that live next to their harnesses being run (in a single chunk) on
    # try, so I can get my results without waiting for the entire suite,
    # and I don't need to sift through logs to figure out which chunk a
    # test lives in when I only care about running certain tests.

For more details run ./mach help try. As the command will inform you,
this feature is under development -- bugs should be filed blocking
[bug 1149670](https://bugzilla.mozilla.org/show_bug.cgi?id=1149670)).

