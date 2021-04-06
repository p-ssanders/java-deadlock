#   java-deadlocker

Very simple implementation of a Java program that deadlocks almost immediately when run.

##  Build

```shell
./gradlew clean build
```

##  Run

```shell
./gradlew run
```

##  Explore

This program deadlocks almost immediately which isn't that interesting if you just run it.
It's a bit more interesting if you debug the application, and watch the deadlock happen.

Try having just `Competitor1` run -- everything will work fine.
Or schedule two instances of either `Competitor1` or `Competitor2` -- everything will work fine.

The deadlock occurs when both `Competitor1` and`Competitor2` run at the same time because they're attempting to obtain
locks on shared resources _in the opposite order_.

Specifically: 
*   `Competitor1` requests a lock on `SHARED_RESOURCE_1` and then `SHARED_RESOURCE_2`
*   `Competitor2` requests a lock on `SHARED_RESOURCE_2` and then `SHARED_RESOURCE_1`

With both `Competitor`s running concurrently, deadlock will occur almost immediately because `Competitor1` holds a lock
on `SHARED_RESOURCE_1` and is waiting for a lock on `SHARED_RESOURCE_2` that it can't get... because `Competitor2` is
holding it.

And `Competitor2` can't release the lock on `SHARED_RESOURCE_2` until it can get a lock on `SHARED_RESOURCE_1`, but it
can't because `Competitor1` is holding it!