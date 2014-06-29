---
layout: post
title: "SSHFS on Mac OS X: Mount process has terminated unexpectedly"
description: ""
category: Mac
tags: [Mac, SSHFS]
---
{% include JB/setup %}

I used to use Macfusion for SSHFS on my old Macbook, but it failed on the new Mountain Lion system. The Macfusion keeps complaining _‘Could not mount filesystem: Mount process has terminated unexpectedly‘_.

I got the SSHFS worked via the command line as is mentioned in [this post](https://code.google.com/p/macfuse/wiki/MACFUSE_FS_SSHFS).

First, download [the sshfs binary](http://osxbook.com/download/sshfs/sshfs-static-leopard.gz) and unzip it. Open the command prompt and change the working directory to your binary location. Run the binary program with your sshfs parameters:

`./sshfs-static-leopard user_name@host:/some_location /some_local_mount_porint`

Enter your password, and you can find your remote directory in your mount point!
