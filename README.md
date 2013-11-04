# OutageFinder

Perform a small number of high-level, client side tests, in parallel, against
FI proxies and parse the results to look for Bad Things.

## What does it do?

OutageFinder performs a number of tests on each host.

In particular, it will do `ping` tests, `traceroute` tests and a simple HTTP
test.

It will do each set of tests in parallel, then report any suspicious results.

## Why?

Because it's annoying having to find out which proxy is causing an issue every
time we get a flood of nagios alerts. If this helps, then yay.

This doesn't test specific apps, and isn't helpful in the case of "pkgdb is
down but only from proxy05." But it should be helpful if a particular proxy
goes entirely down (or gets really slow), and we're not sure which one.

The root problem is that our infrastructure talks to itself over the main
round-robin DNS rotation. So by the time we start getting alerts, they are
coming from everywhere that we host apps, because the apps are fataling as
they try to talk to the downed proxy. The alerts become useless because every
app server starts throwing alerts, which causes the proxies to throw alerts
which makes finding the individual proxy with the **real problem** really hard.

In this scenario, run `outagefinder` and wait a few seconds. With
some luck it'll tell you which proxy is acting up.


## License

&copy; 2013 Red Hat, Inc.

GPLv2+.
