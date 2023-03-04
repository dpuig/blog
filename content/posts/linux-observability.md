+++
title = "Linux Observability - uptime"
date = "2023-02-24T15:10:30-05:00"
author = "Daniel Puig"
authorTwitter = "dpuig" #do not include @
cover = ""
tags = ["linux", "observability", "monitoring", "cli", "commands"]
keywords = ["", ""]
description = "In the context of Linux operating systems, observability and monitoring involve using tools and techniques to collect and analyze data about system behavior and performance."
showFullContent = false
readingTime = false
hideComments = false
color = "green" #color from the theme settings
+++

In the world of modern software development, observability and monitoring have become critical components for ensuring the reliability and performance of systems. Linux, as one of the most widely used operating systems, is no exception to this trend. In this post, we will explore the concepts of observability and monitoring in the context of Linux operating systems. We will discuss the importance of these concepts for Linux administrators and developers, the tools and techniques available for monitoring and observability, and best practices for implementing observability and monitoring in a Linux environment. Whether you are a seasoned Linux professional or just starting out, understanding observability and monitoring in the Linux context is essential for maintaining the health and reliability of your systems.

## uptime

The `uptime` command in Linux is used to display the current time, how long the system has been running, the number of users currently logged in, and the system load averages for the past 1, 5, and 15 minutes.

```sh
$ uptime -h                                                        

Usage:
 uptime [options]

Options:
 -p, --pretty   show uptime in pretty format
 -h, --help     display this help and exit
 -s, --since    system up since
 -V, --version  output version information and exit

For more details see uptime(1).
```

```sh
$ uptime

15:31:46 up 7 days, 19:04,  1 user,  load average: 0.58, 0.65, 0.56
```

- The current time – 15:31:46
- How long the system has been running – up 7 days
- How many users are currently logged on – 1 user
- The system load averages for the past 1, 5, and 15 minutes (0.15, 0.08, 0.08)

__Pretty__

```sh
$ uptime -p

up 1 week, 19 hours, 7 minutes
```

__Since__

```sh
$ uptime -s

2023-02-23 20:27:17
```

`w` making use of `uptime`, displays information about currently logged in users, their processes and system uptime. When you run the w command, it outputs the following information:

- The current time
- How long the system has been running (uptime)
- The number of users currently logged in
- A list of logged in users, along with their login time, remote host address (if any), and the processes they are running

The output of the w command is useful for system administrators who need to keep track of the current system usage and who is currently logged in.

Here is an example output of the w command:

```sh
14:38:59 up 10 days,  3:23,  3 users,  load average: 0.05, 0.02, 0.01
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
john     pts/0    192.168.0.2      14:22    1:07   0.02s  0.02s sshd: john@pts/0
jane     pts/1    192.168.0.3      09:20    4:48   0.13s  0.13s -bash
admin    pts/2    192.168.0.4      11:33    2:01   0.05s  0.05s top
```


