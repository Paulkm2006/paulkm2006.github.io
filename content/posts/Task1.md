+++ 
draft = false
date = 2024-10-23T14:46:13+08:00
title = "Task1 Notes"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

# What is 'context'

In go-redis, all function calls requires a additional parameter called 'context', which should be simply predefined like this
```go
ctx := context.Background()
codeInRedis, err := RedisClient.Get(ctx, email).Result()
```
Upon reading some documents, I found that the context being forwarded here, `context.Background()` does'n t seem to contain any meaningful action. However, the package `context` itself is quite useful.

E.g. suppose we're calling a function that should either return a value or end after a timeout. Normally, we can achieve that by passing a timer, and handle the timeout internally in the goroutime.

However, we can also pass a `context.WithDeadline`. This generates a channel for the goroutine to continuously check whether it should break, and (if there's any,) why it should be ended.

Since we just want the data we provided to be read/written into Redis server, so a simple no-constraint background should be enough.

[reference](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/Context.html)

# Shell pipeline & redirection
Linux shell offers its users some great tools named pipeline and redirection. Basically, it allows us to redirect output of one command to the input of another command. Note that the operator < > and >> supports file descriptors (which means that it can be used on file operations).

### The `|` operator
This operator simply redirect the output of its first parameter to the input of its second parameter.
E.g. `ls | grep test` means take the output of `ls` command as the input of `grep` command, and print out the final result from `grep` to the console.
| can also be used in chain, meaning that we can form a chain of processing to obtain the final result.
E.g. `ls | grep test | sort` returns the filtered and sorted result of `ls`.
Since pipelines are handled by kernel in memory buffer, it doesn't involve any disk operation or requires additional configuration by software developers.

### The `>` and `>>` operator
This operator redirects stdout to a file. Note that > overrides the file if exist, while >> append to the file.
If we want to redirect stderr and stdout into the same file, we can use `cmd > file 2>&1`. It combines stderr(2) with stdout(1).
Also, we can separate stderr and stdout into different files like `cmd 1>out 2>err`
We can use `cmd > /dev/null` to 'silence' the command. (redirect its stdout to null so it won't print anything on console)

### The `<` operator
This operator redirects a file as the stdin of the command. 

### The `<<` operator (here document)
We can use << to send a multi-line parameter to a command. 
E.g. 
```shell
wc -l <<EOF
test1
test2
EOF
```
will output 2 