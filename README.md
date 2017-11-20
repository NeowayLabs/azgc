# Azure Garbage Collector

Performs garbage collection on your azure subscription.
What would be garbage ? Any resource group that exceeded
its time to live.

## Why

There is two main cases where we can left garbage on a azure
subscription:

* Some automated testing that went wrong
* Some automated backup validation that went wrong

Being completely sure that these operations never leak
anything (think catastrophic failures) has not worked out
for us, so we created this project to help us cleanup
resource groups that where meant to live just a fixed
time, the time that it takes to perform the testing.

## How

The idea is pretty basic, lets just delete all resource
groups that have a name matching a pattern and that is
alive for more than **X** time (**X** is the time to live).

It is VERY important to choose a good name pattern for your
resource groups that are used for testing, if the pattern match
with something that you DON'T want to delete you are going
to have a bad time.

## Install

Simple as any Go tool:

```
go get github.com/NeowayLabs/azgc
```

## Usage

You'll need to export the environment variables below:

* AZURE_SUBSCRIPTION_ID
* AZURE_TENANT_ID
* AZURE_CLIENT_ID
* AZURE_CLIENT_SECRET

And run:

```
azgc -resgroups <resgroup name regex> -ttl <golang time>
```

If you think that running this manually is a bad idea you
are probably right. If you have a cluster orchestration tool
like Kubernetes you could use a cron job on it to run this
periodically (Kubernetes secrets can help you injecting
the variables too).
