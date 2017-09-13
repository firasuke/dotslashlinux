+++
title = "The Linux Kernel Configuration Guide Part 14"
slug = "the linux kernel configuration guide part 14"
nick = "kernel14"
date = "2017-09-14"
author = "Firas Khalil Khana"
imgsrc = "/img/kernel14.png"
imgalt = "kernel14"
categories = [ "kernel" ]
+++
<h3>Kernel hacking  ---></h3>
<h3>printk and dmesg options  ---></h3>
<h3>[&ast;] Show timing information on printks</h3>
```none
Symbol:     CONFIG_PRINTK_TIME

Help:       Selecting this option causes time stamps of the printk()
            messages to be added to the output of the syslog() system
            call and at the console.

            The timestamp is always recorded internally, and exported
            to /dev/kmsg. This flag just specifies if the timestamp should
            be included, not that the timestamp is recorded.

            The behavior is also controlled by the kernel command line
            parameter printk.time=1. See Documentation/admin-guide/kernel-parameters.rst

Type:       boolean

Choice:     built-in [*]

Reason:     It's highly recommended that you include this option in your kernel
            as timestamps add another level of precision to your system logs.
```
<h3>(15) Default console loglevel (1-15)</h3>
```none
Symbol:     CONFIG_CONSOLE_LOGLEVEL_DEFAULT

Help:       Default loglevel to determine what will be printed on the console.

            Setting a default here is equivalent to passing in loglevel=<x> in
            the kernel bootargs. loglevel=<x> continues to override whatever
            value is specified here as well.

            Note: This does not affect the log level of un-prefixed printk()
            usage in the kernel. That is controlled by the MESSAGE_LOGLEVEL_DEFAULT
            option.

Type:       integer

Choice:     (15) custom

Reason:     It's recommended that you set the value of this option to a value
            that ensures enough verbosity is being used.

            If you've already excluded a bunch of debugging options to minimize
            system overhead, then you'd likely want to set the value of this
            option and CONFIG_MESSAGE_LOGLEVEL_DEFAULT to a high enough value
            to get enough information.
```
<h3>(7) Default message log level (1-7)</h3>
```none
Symbol:     CONFIG_MESSAGE_LOGLEVEL_DEFAULT

Help:       Default log level for printk statements with no specified priority.

            This was hard-coded to KERN_WARNING since at least 2.6.10 but folks
            that are auditing their logs closely may want to set it to a lower
            priority.

            Note: This does not affect what message level gets printed on the console
            by default. To change that, use loglevel=<x> in the kernel bootargs,
            or pick a different CONSOLE_LOGLEVEL_DEFAULT configuration value.

Type:       integer

Choice:     (7) custom

Reason:     It's recommended that you set the value of this option to a value
            that ensures enough verbosity is being used.

            If you've already excluded a bunch of debugging options to minimize
            system overhead, then you'd likely want to set the value of this
            option and CONFIG_CONSOLE_LOGLEVEL_DEFAULT to a high enough value
            to get enough information.
```
<h3>[&ast;] Enable dynamic printk() support</h3>
```none
Symbol:     CONFIG_DYNAMIC_DEBUG

Help:       Compiles debug level messages into the kernel, which would not
            otherwise be available at runtime. These messages can then be
            enabled/disabled based on various levels of scope - per source file,
            function, module, format string, and line number. This mechanism
            implicitly compiles in all pr_debug() and dev_dbg() calls, which
            enlarges the kernel text size by about 2%.

            If a source file is compiled with DEBUG flag set, any
            pr_debug() calls in it are enabled by default, but can be
            disabled at runtime as below.  Note that DEBUG flag is
            turned on by many CONFIG_*DEBUG* options.
            
            Usage:

            Dynamic debugging is controlled via the 'dynamic_debug/control' file,
            which is contained in the 'debugfs' filesystem. Thus, the debugfs
            filesystem must first be mounted before making use of this feature.
            We refer the control file as: <debugfs>/dynamic_debug/control. This
            file contains a list of the debug statements that can be enabled. The
            format for each line of the file is:

                  filename:lineno [module]function flags format

            filename : source file of the debug statement
            lineno : line number of the debug statement
            module : module that contains the debug statement
            function : function that contains the debug statement
            flags : '=p' means the line is turned 'on' for printing
            format : the format used for the debug statement

            From a live system:
       
                  nullarbor:~ # cat <debugfs>/dynamic_debug/control
                  # filename:lineno [module]function flags format
                  fs/aio.c:222 [aio]__put_ioctx =_ "__put_ioctx:\040freeing\040%p\012"
                  fs/aio.c:248 [aio]ioctx_alloc =_ "ENOMEM:\040nr_events\040too\040high\012"
                  fs/aio.c:1770 [aio]sys_io_cancel =_ "calling\040cancel\012"

            Example usage:
      
                  // enable the message at line 1603 of file svcsock.c
                  nullarbor:~ # echo -n 'file svcsock.c line 1603 +>
                                                  <debugfs>/dynamic_debug/control
      
                  // enable all the messages in file svcsock.c
                  nullarbor:~ # echo -n 'file svcsock.c +p' >
                                                  <debugfs>/dynamic_debug/control
      
                  // enable all the messages in the NFS server module
                  nullarbor:~ # echo -n 'module nfsd +p' >
                                                  <debugfs>/dynamic_debug/control
      
                  // enable all 12 messages in the function svc_process()
                  nullarbor:~ # echo -n 'func svc_process +p' >
                                                  <debugfs>/dynamic_debug/control
      
                  // disable all 12 messages in the function svc_process()
                  nullarbor:~ # echo -n 'func svc_process -p' >
                                                  <debugfs>/dynamic_debug/control

            See Documentation/admin-guide/dynamic-debug-howto.rst for additional
            information.

Type:       boolean

Choice:     built-in [*]

Reason:     You can safely exclude this option along with CONFIG_DEBUG_FS if you
            have no need for dynamic kernel debugging.
```
<h3>Compile-time checks and compiler options  ---></h3>
<h3>(0) Warn for stack frames larger than (needs gcc 4.4)</h3>
```none
Symbol:     CONFIG_FRAME_WARN

Help:       Tell gcc to warn at build time for stack frames larger than this.
            Setting this too low will cause a lot of warnings.
            Setting it to 0 disables the warning.
            Requires gcc 4.4 

Type:       integer

Choice:     (0) custom

Reason:     If you can live with the warnings then don't disable this option.
```
<h3>[&ast;] Debug Filesystem</h3>
```none
Symbol:     CONFIG_DEBUG_FS

Help:       debugfs is a virtual file system that kernel developers use to put
            debugging files into.  Enable this option to be able to read and
            write to these files.

            For detailed documentation on the debugfs API, see
            Documentation/DocBook/filesystems.

            If unsure, say N.

Type:       boolean

Choice:     built-in [*]

Reason:     It's required that you include this option if you plan on using
            dynamic kernel debugging (forcibly included by CONFIG_DYNAMIC_DEBUG).
```
<h3>-&ast;- Kernel debugging</h3>
```none
Symbol:     CONFIG_DEBUG_KERNEL

Help:       Say Y here if you are developing drivers or trying to debug and
            identify kernel problems.

Type:       boolean

Choice:     built-in -*-

Reason:     This option is forcibly included by CONFIG_EXPERT.
```
<h3>(0) panic timeout</h3>
```none
Symbol:     CONFIG_PANIC_TIMEOUT

Help:       Set the timeout value (in seconds) until a reboot occurs when the
            the kernel panics. If n = 0, then we wait forever. A timeout
            value n > 0 will wait n seconds before rebooting, while a timeout
            value n < 0 will reboot immediately.

Type:       integer

Choice:     (0) custom

Reason:     Setting the value of this option to (0) will give you all the time
            you want to carefully examine the cause of the kernel panic.
```
<h3>RCU Debugging  ---></h3>
<h3>(3) RCU CPU stall timeout in seconds</h3>
```none
Symbol:     CONFIG_RCU_CPU_STALL_TIMEOUT

Help:       If a given RCU grace period extends more than the specified
            number of seconds, a CPU stall warning is printed.  If the
            RCU grace period persists, additional CPU stall warnings are
            printed at more widely spaced intervals.

Type:       integer

Choice:     (3) custom
```
<h3>IO delay type (no port-IO delay)  ---></h3>
<h3>(X) no port-IO delay</h3>
```none
Symbol:     CONFIG_IO_DELAY_NONE

Help:       No port-IO delay. Will break on old boxes that require port-IO
            delay for certain operations. Should work on most new machines.

Type:       boolean

Choice:     built-in (X)

Reason:     It's recommended that you include this option if you're on a modern
            system.
```