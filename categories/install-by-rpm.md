::: {#main .section}
::: {#page}
::: {.topic_content}
::: {style="text-align:right"}
::: {style="text-align:right"}
Versions \| [v1.0 (td-agent3)](/v1.0/articles/install-by-rpm) \|
***v0.12* (td-agent2) **
:::
:::

------------------------------------------------------------------------

Installing Fluentd Using rpm Package
====================================

This article explains how to install the td-agent rpm package, the
stable Fluentd distribution package maintained by [Treasure Data,
Inc](http://www.treasuredata.com/).

[]{#what-is-td-agent?}

::: {#table-of-contents .section}
### Table of Contents

-   [What is td-agent?](#what-is-td-agent?)
-   [Step 0: Before Installation](#step-0:-before-installation)
-   [Step 1: Install from rpm
    Repository](#step-1:-install-from-rpm-repository)
-   [Step2: Launch Daemon](#step2:-launch-daemon)
-   [Step3: Post Sample Logs via
    HTTP](#step3:-post-sample-logs-via-http)
-   [Next Steps](#next-steps)
:::

What is td-agent?
-----------------

Fluentd is written in Ruby for flexibility, with performance sensitive
parts written in C. However, casual users may have difficulty installing
and operating a Ruby daemon.

That's why [Treasure Data, Inc](http://www.treasuredata.com/) is
providing **the stable community distribution of Fluentd**, called
`td-agent`. The differences between Fluentd and td-agent can be found
[here](//www.fluentd.org/faqs).

Currently, td-agent 2 rpm has two versions, td-agent 2.5 / td-agent 2.3.
The different point is bundled ruby version. td-agent 2.5 or later uses
ruby 2.5 and td-agent 2.3 or earlier uses ruby 2.1. ruby 2.1 is EOL so
we recommend to use td-agent 2.5 for new deployment. td-agent 2.5 and
td-agent 2.3 use fluentd v0.12 serise so the behaviour is same.

[]{#step-0:-before-installation}

Step 0: Before Installation
---------------------------

Please follow the [Preinstallation Guide](before-install) to configure
your OS properly. This will prevent many unnecessary problems.

[]{#step-1:-install-from-rpm-repository}

Step 1: Install from rpm Repository
-----------------------------------

CentOS and RHEL 5, 6, 7 and Amazon Linux are currently supported.

Executing
[install-redhat-td-agent2.sh](https://toolbelt.treasuredata.com/sh/install-redhat-td-agent2.sh)
will automatically install td-agent on your machine. This shell script
registers a new rpm repository at `/etc/yum.repos.d/td.repo` and
installs the `td-agent` rpm package.

``` {.CodeRay}
# td-agent 2.5 or later. Only CentOS/RHEL 6 and 7 for now.
$ curl -L https://toolbelt.treasuredata.com/sh/install-redhat-td-agent2.5.sh | sh
# td-agent 2.3 or earlier
$ curl -L https://toolbelt.treasuredata.com/sh/install-redhat-td-agent2.sh | sh
```

It's HIGHLY recommended that you set up **ntpd** on the node to prevent
invalid timestamps in your logs. Please check the [Preinstallation
Guide](before-install).
:::
:::
:::

td-agent supports recent 2 Amazon Linux versions. It means if the latest
version is 2016.09, we provides new packages for 2016.03 and 2016.09. No
new packages for 2015.09 or earlier.

[]{#step2:-launch-daemon}

Step2: Launch Daemon
--------------------

The `/etc/init.d/td-agent` script is provided to start, stop, or restart
the agent.

``` {.CodeRay}
$ sudo /etc/init.d/td-agent start
Starting td-agent: [  OK  ]
$ sudo /etc/init.d/td-agent status
td-agent (pid  21678) is running...
```

The following commands are supported:

``` {.CodeRay}
$ sudo /etc/init.d/td-agent start
$ sudo /etc/init.d/td-agent stop
$ sudo /etc/init.d/td-agent restart
$ sudo /etc/init.d/td-agent status
```

Please make sure **your configuration file** is located at
`/etc/td-agent/td-agent.conf`.

[]{#step3:-post-sample-logs-via-http}

Step3: Post Sample Logs via HTTP
--------------------------------

By default, `/etc/td-agent/td-agent.conf` is configured to take logs
from HTTP and route them to stdout (`/var/log/td-agent/td-agent.log`).
You can post sample log records using the curl command.

``` {.CodeRay}
$ curl -X POST -d 'json={"json":"message"}' http://localhost:8888/debug.test
```

[]{#next-steps}

Next Steps
----------

You're now ready to collect your real logs using Fluentd. Please see the
following tutorials to learn how to collect your data from various data
sources.

-   Basic Configuration
    -   [Config File](config-file)
-   Application Logs
    -   [Ruby](ruby), [Java](java), [Python](python), [PHP](php),
        [Perl](perl), [Node.js](nodejs), [Scala](scala)
-   Examples
    -   [Store Apache Log into Amazon S3](apache-to-s3)
    -   [Store Apache Log into MongoDB](apache-to-mongodb)
    -   [Data Collection into HDFS](http-to-hdfs)

Please refer to the resources below for further steps.

-   [ChangeLog of
    td-agent](http://docs.treasuredata.com/articles/td-agent-changelog)
-   [Chef Cookbook](https://github.com/treasure-data/chef-td-agent/)

::: {style="text-align:right"}
Last updated: 2017-01-04 10:42:53 UTC
:::

------------------------------------------------------------------------

::: {style="text-align:right"}
Versions \| [v1.0 (td-agent3)](/v1.0/articles/install-by-rpm) \|
***v0.12* (td-agent2) **
:::

------------------------------------------------------------------------

If this article is incorrect or outdated, or omits critical information,
please [let us
know](https://github.com/fluent/fluentd-docs/issues?state=open).
[Fluentd](http://www.fluentd.org/) is a open source project under [Cloud
Native Computing Foundation (CNCF)](https://cncf.io/). All components
are available under the Apache 2 License.