+++
title = "The Linux Kernel Configuration Guide Part 15"
slug = "the linux kernel configuration guide part 15"
nick = "kernel15"
date = "2017-09-15"
author = "Firas Khalil Khana"
imgsrc = "/img/kernel15.png"
imgalt = "kernel15"
categories = [ "kernel" ]
+++
While security is important, it isn't a high priority in this series (although we've gone through some options related to security).
<br/>
<br/>
You know what they say "There isn't a 100% secure system". You have to find the right balance between conveniency, usability and security otherwise you can easily render a system unusable if you beefed security up to an insane level.
<br/>
<br/>
I'd recommend (at least as a starting point) that you leave all options in this section excluded (or only include those required by other options).
<hr/>
<h3>-&ast;- Enable the securityfs filesystem</h3>
```none
Symbol:     CONFIG_SECURITYFS

Help:       This will build the securityfs filesystem.  It is currently used by
            the TPM bios character driver and IMA, an integrity provider.  It is
            not used by SELinux or SMACK.

            If you are unsure how to answer this question, answer N.

Type:       boolean

Choice:     built-in -*-

Reason:     You can exclude this option if it wasn't required by any other option.

            (In my case it was required by CONFIG_TCG_TPM)
```
<h3>Default security module (Unix Discretionary Access Controls)  ---></h3>
```none
Help:       Select the security module that will be used by default if the
            kernel parameter security= is not specified.
```
<h3>(X) Unix Discretionary Access Controls</h3>
```none
Symbol:     CONFIG_DEFAULT_SECURITY_DAC

Help:       There is no help available for this option.

Type:       boolean

Choice:     built-in (X)

Reason:     Only option left =D
```