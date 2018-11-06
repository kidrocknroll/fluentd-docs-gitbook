::: {#main .section}
::: {#page}
::: {.topic_content}
::: {style="text-align:right"}
::: {style="text-align:right"}
Versions \| ***v0.12* (td-agent2) **
:::
:::

------------------------------------------------------------------------

Multiprocess Input Plugin
=========================

By default, Fluentd only uses a single CPU core on the system. The
`in_multiprocess` Input plugin enables Fluentd to use multiple CPU cores
by spawning multiple child processes. One Fluentd user is using this
plugin to handle 10+ billion records / day.

[]{#install}

::: {#table-of-contents .section}
### Table of Contents

[Install](#install)

[Example Configuration](#example-configuration)

[Parameters](#parameters)

-   [\@type (required)](#@type-(required))
-   [graceful\_kill\_interval](#graceful_kill_interval)
-   [graceful\_kill\_interval\_increment](#graceful_kill_interval_increment)
-   [graceful\_kill\_timeout](#graceful_kill_timeout)
-   [process (required)](#process-(required))
-   [cmdline (required)](#cmdline-(required))
-   [sleep\_before\_start](#sleep_before_start)
-   [sleep\_before\_shutdown](#sleep_before_shutdown)
:::

Install
-------

`in_multiprocess` is NOT included in td-agent by default. td-agent users
must install fluent-plugin-multiprocess manually.

-   Fluentd: `fluent-gem install fluent-plugin-multiprocess`
-   td-agent v2:
    `/usr/sbin/td-agent-gem install fluent-plugin-multiprocess`
-   td-agent v1:
    `/usr/lib/fluent/ruby/bin/fluent-gem install fluent-plugin-multiprocess`

[]{#example-configuration}

Example Configuration
---------------------

``` {.CodeRay}
<source>
  @type multiprocess

  <process>
    cmdline -c /etc/fluent/fluentd_child1.conf --log /var/log/fluent/fluentd_child1.log
    sleep_before_start 1s
    sleep_before_shutdown 5s
  </process>
  <process>
    cmdline -c /etc/fluent/fluentd_child2.conf --log /var/log/fluent/fluentd_child2.log
    sleep_before_start 1s
    sleep_before_shutdown 5s
  </process>
  <process>
    cmdline -c /etc/fluent/fluentd_child3.conf --log /var/log/fluent/fluentd_child3.log
    sleep_before_start 1s
    sleep_before_shutdown 5s
  </process>
</source>
```
:::
:::
:::

Please see the [Config File](config-file) article for the basic
structure and syntax of the configuration file.

Especially for daemonized fluentd (ex: td-agent), \`\--log\` option MUST
be specified to put logs of child processes.

[]{#parameters}

Parameters
----------

[]{#@type-(required)}

### \@type (required)

The value must be `multiprocess`.

[]{#graceful_kill_interval}

### graceful\_kill\_interval

The interval to send the signal to gracefully shut down the process
(default: 2sec).

[]{#graceful_kill_interval_increment}

### graceful\_kill\_interval\_increment

The increment time, when graceful shutdown fails (default: 3sec).

[]{#graceful_kill_timeout}

### graceful\_kill\_timeout

The timeout, to identify the failure of graceful shutdown (default:
60sec).

[]{#process-(required)}

### process (required)

The `process` section sets the command line arguments of a child
process. This plugin creates one child process for each section.

[]{#cmdline-(required)}

### cmdline (required)

The `cmdline` option is required in a section

[]{#sleep_before_start}

### sleep\_before\_start

This parameter sets the wait time before starting the process (default:
0sec).

[]{#sleep_before_shutdown}

### sleep\_before\_shutdown

This parameter sets the wait time before shutting down the process
(default: 0sec).

#### log\_level option

The `log_level` option allows the user to set different levels of
logging for each plugin. The supported log levels are: `fatal`, `error`,
`warn`, `info`, `debug`, and `trace`.

Please see the [logging article](logging) for further details.

::: {style="text-align:right"}
Last updated: 2016-10-21 06:29:58 UTC
:::

------------------------------------------------------------------------

::: {style="text-align:right"}
Versions \| ***v0.12* (td-agent2) **
:::

------------------------------------------------------------------------

If this article is incorrect or outdated, or omits critical information,
please [let us
know](https://github.com/fluent/fluentd-docs/issues?state=open).
[Fluentd](http://www.fluentd.org/) is a open source project under [Cloud
Native Computing Foundation (CNCF)](https://cncf.io/). All components
are available under the Apache 2 License.