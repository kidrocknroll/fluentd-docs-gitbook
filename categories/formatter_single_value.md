::: {#main .section}
::: {#page}
::: {.topic_content}
::: {style="text-align:right"}
::: {style="text-align:right"}
Versions \| [v1.0 (td-agent3)](/v1.0/articles/formatter_single_value) \|
***v0.12* (td-agent2) **
:::
:::

------------------------------------------------------------------------

single\_value Formatter Plugin
==============================

The `single_value` formatter plugin output the value of a single field
instead of the whole record.

This formatter is often used in conjunction with [in\_tail](in_tail)'s
`format none`.

[]{#parameters}

::: {#table-of-contents .section}
### Table of Contents

[Parameters](#parameters)

-   [add\_newline (Boolean, Optional, defaults to
    true)](#add_newline-(boolean,-optional,-defaults-to-true))
-   [message\_key (String, Optional, defaults to
    "message")](#message_key-(string,-optional,-defaults-to-%E2%80%9Cmessage%E2%80%9D))

[Example](#example)
:::

Parameters
----------

[]{#add_newline-(boolean,-optional,-defaults-to-true)}

### add\_newline (Boolean, Optional, defaults to true)

Add `\n` to the result. If there is a trailing "\\n" already, set it
"false"

[]{#message_key-(string,-optional,-defaults-to-%E2%80%9Cmessage%E2%80%9D)}

### message\_key (String, Optional, defaults to "message")

The value of this field is outputted.

[]{#example}

Example
-------

``` {.CodeRay}
tag:    app.event
time:   1362020400
record: {"message":"Hello from Fluentd!"}
```

This incoming event is formatted to:

``` {.CodeRay}
Hello from Fluentd!\n
```

::: {style="text-align:right"}
Last updated: 2018-11-06 18:17:13 +0000
:::

------------------------------------------------------------------------

::: {style="text-align:right"}
Versions \| [v1.0 (td-agent3)](/v1.0/articles/formatter_single_value) \|
***v0.12* (td-agent2) **
:::

------------------------------------------------------------------------

If this article is incorrect or outdated, or omits critical information,
please [let us
know](https://github.com/fluent/fluentd-docs/issues?state=open).
[Fluentd](http://www.fluentd.org/) is a open source project under [Cloud
Native Computing Foundation (CNCF)](https://cncf.io/). All components
are available under the Apache 2 License.
:::
:::
:::