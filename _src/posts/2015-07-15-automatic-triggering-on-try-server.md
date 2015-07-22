    Title: Automatic triggering on try server
    Date: 2015-07-15T14:44:13
    Tags: mozilla

If you've pushed to Mozilla's try server recently, you've probably noticed
some of your failures being re-triggered automatically. This happens courtesy
of the [trigger bot](https://github.com/chmanchester/trigger-bot) tool
implemented as a pulse listener that invokes self-serve in response to certain
events.

Two trigerring rules are currently implemented:

* When a test fails it's triggered one more time, up to a certain portion of all
  jobs for the push.
* When ```--rebuild``` is found in try syntax, all test jobs for that push are
  triggered the specified number of times.

Finally, if ```--rebuild``` is specified, nothing gets triggered when a test fails,
and if ```--no-retry``` is found in try syntax, no triggering happens for the push.

This was enabled at the end of June, and I've gotten a bit of feedback
(almost mostly positive). My own curiosity is about the overall volume of
these jobs, so I pulled some stats from self-serve to see how much we've ended up
triggering. For the first 2 weeks of July, there were **373,772** jobs on try,
**13,099** of which were initiated by the trigger-bot (**3.5%**).

This is a larger proportion than I think it should be, but we've recently landed
changes that mean we will trigger fewer hidden jobs. Hidden jobs are usually
hidden due to failing persistently, so this should reduce overall triggering.

The rudimentary script I used to gather these stats is available on the
[trigger-bot repo](https://github.com/chmanchester/trigger-bot/commit/845bdf3737b5c4bff2a38c45311f71c96ec450dc)


<!-- more -->
