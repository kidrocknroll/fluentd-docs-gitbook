::: {#main .section}
::: {#page}
::: {.topic_content}
::: {style="text-align:right"}
::: {style="text-align:right"}
Versions \| [v1.0 (td-agent3)](/v1.0/articles/parser_multiline) \|
***v0.12* (td-agent2) **
:::
:::

------------------------------------------------------------------------

multiline Parser Plugin
=======================

The `multiline` parser plugin parses multiline logs. This plugin is
multiline version of `regexp` parser.

The `multiline` parser parses log with `formatN` and `format_firstline`
parameters. `format_firstline` is for detecting start line of multiline
log. `formatN`, N's range is 1..20, is the list of Regexp format for
multiline log.
:::
:::
:::

Unlike other parser plugins, this plugin needs special code in input
plugin, e.g. handle format\_firstline. So currently, in\_tail plugin
works with \`multiline\` but other input plugins don\'t work with
\`multiline\`.

[]{#parameters}

::: {#table-of-contents .section}
### Table of Contents

[Parameters](#parameters)

-   [time\_key](#time_key)
-   [time\_format](#time_format)
-   [format\_firstline](#format_firstline)
-   [formatN](#formatn)
-   [keep\_time\_key](#keep_time_key)

[Example](#example)

-   [Rails log](#rails-log)
-   [Java stacktrace log](#java-stacktrace-log)
:::

Parameters
----------

[]{#time_key}

### time\_key

Specify the field for event time. Default is `time`.

[]{#time_format}

### time\_format

Specify time format for `time_key`.

See
[Time\#strptime](http://ruby-doc.org/stdlib-2.4.1/libdoc/time/rdoc/Time.html#method-c-strptime)
for additional format information.

[]{#format_firstline}

### format\_firstline

Specify regexp pattern for start line of multiple lines. Input plugin
can skip the logs until `format_firstline` is matched. Default is `nil`.

If `format_firstline` is not specified, input plugin should store
unmatched new lines in temporary buffer and try to match buffered logs
with each new line.

[]{#formatn}

### formatN

Specify regexp patterns. For readability, you can separate regexp
patterns into multiple regexpN parameters, See "Rails log" example.
These patterns are joined and constructs regexp pattern with multiline
mode.

[]{#keep_time_key}

### keep\_time\_key

If you want to keep time field in the record, set `true`. Default is
`false`.

[]{#example}

Example
-------

[]{#rails-log}

### Rails log

``` {.CodeRay}
format multiline
format_firstline /^Started/
format1 /Started (?<method>[^ ]+) "(?<path>[^"]+)" for (?<host>[^ ]+) at (?<time>[^ ]+ [^ ]+ [^ ]+)\n/
format2 /Processing by (?<controller>[^\u0023]+)\u0023(?<controller_method>[^ ]+) as (?<format>[^ ]+?)\n/
format3 /(  Parameters: (?<parameters>[^ ]+)\n)?/
format4 /  Rendered (?<template>[^ ]+) within (?<layout>.+) \([\d\.]+ms\)\n/
format5 /Completed (?<code>[^ ]+) [^ ]+ in (?<runtime>[\d\.]+)ms \(Views: (?<view_runtime>[\d\.]+)ms \| ActiveRecord: (?<ar_runtime>[\d\.]+)ms\)/
```

With this configuration:

``` {.CodeRay}
Started GET "/users/123/" for 127.0.0.1 at 2013-06-14 12:00:11 +0900
Processing by UsersController#show as HTML
  Parameters: {"user_id"=>"123"}
  Rendered users/show.html.erb within layouts/application (0.3ms)
Completed 200 OK in 4ms (Views: 3.2ms | ActiveRecord: 0.0ms)
```

This incoming event is parsed as:

``` {.CodeRay}
time:
1371178811 (2013-06-14 12:00:11 +0900)

record:
{
  "method"           :"GET",
  "path"             :"/users/123/",
  "host"             :"127.0.0.1",
  "controller"       :"UsersController",
  "controller_method":"show",
  "format"           :"HTML",
  "parameters"       :"{ \"user_id\":\"123\"}",
  ...
}
```

[]{#java-stacktrace-log}

### Java stacktrace log

``` {.CodeRay}
format multiline
format_firstline /\d{4}-\d{1,2}-\d{1,2}/
format1 /^(?<time>\d{4}-\d{1,2}-\d{1,2} \d{1,2}:\d{1,2}:\d{1,2}) \[(?<thread>.*)\] (?<level>[^\s]+)(?<message>.*)/
```

With this configuration:

``` {.CodeRay}
2013-3-03 14:27:33 [main] INFO  Main - Start
2013-3-03 14:27:33 [main] ERROR Main - Exception
javax.management.RuntimeErrorException: null
    at Main.main(Main.java:16) ~[bin/:na]
2013-3-03 14:27:33 [main] INFO  Main - End
```

These incoming events are parsed as:

``` {.CodeRay}
time:
2013-03-03 14:27:33 +0900
record:
{
  "thread" :"main",
  "level"  :"INFO",
  "message":"  Main - Start"
}

time:
2013-03-03 14:27:33 +0900
record:
{
  "thread" :"main",
  "level"  :"ERROR",
  "message":" Main - Exception\njavax.management.RuntimeErrorException: null\n    at Main.main(Main.java:16) ~[bin/:na]"
}

time:
2013-03-03 14:27:33 +0900
record:
{
  "thread" :"main",
  "level"  :"INFO",
  "message":"  Main - End"
}
```

::: {style="text-align:right"}
Last updated: 2018-11-06 18:16:45 +0000
:::

------------------------------------------------------------------------

::: {style="text-align:right"}
Versions \| [v1.0 (td-agent3)](/v1.0/articles/parser_multiline) \|
***v0.12* (td-agent2) **
:::

------------------------------------------------------------------------

If this article is incorrect or outdated, or omits critical information,
please [let us
know](https://github.com/fluent/fluentd-docs/issues?state=open).
[Fluentd](http://www.fluentd.org/) is a open source project under [Cloud
Native Computing Foundation (CNCF)](https://cncf.io/). All components
are available under the Apache 2 License.