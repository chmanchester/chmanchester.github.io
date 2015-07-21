    Title: Automatic triggering on try server
    Date: 2015-07-15T14:44:13
    Tags: DRAFT

If you've pushed to Mozilla's try server recently, you've probably noticed
some of your failures being re-triggered automatically. This happens courtesy
of the trigger bot tool implemented as a pulse listener that invokes self-serve
in response to certain events. When a test fails that's visible according to
Treeherder, it's triggered one more time, up to a certain portion of all jobs
for the push, and when --rebuild is found in try syntax, all test jobs for that
push are triggered the specified number of times. If --rebuild is specified,
nothing gets triggered when a test fails, and if --no-retry is found in try
syntax, no triggering happens for the push.

This was all enabled at the end of June, and I've gotten a bit of feedback
(mostly positive). My own curiosity is about the cost of triggering all these
jobs, so I pulled some stats from self-serve to see how much we've ended up
triggering.

<!-- more -->
