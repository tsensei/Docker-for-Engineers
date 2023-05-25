---
description: Change root. Jail a process.
---

# chroot

There might arise a situation where we want to isolate certain applications, user or environments within our OS. In Linux, the classic way it through using `chroot` enviroment.

A `chroot` environment is an operating system call that will change the root location temporarily to a new folder during the duration of that chroot. This directory will serve as the top level directory for that duration.

Any applications that are run from within the `chroot` will be unable to see the rest of the operating system in principle. Similarly, a non-root user who is confined to a chroot environment will not be able to move further up the directory hierarchy.&#x20;

### When to use chroot

We can take chroot as a way to create a sandboxed OS for testing. It functions like a virtual machine but is a lighter solution without the need of any hypervisor. It resides in our operation system, shares the existing kernel and uses a directory of our file system as its root. So any changes, be it malicious, won't affect our base OS.&#x20;



### References used :&#x20;

{% embed url="https://frontendmasters.com/courses/complete-intro-containers/chroot/" %}

{% embed url="https://www.digitalocean.com/community/tutorials/how-to-configure-chroot-environments-for-testing-on-an-ubuntu-12-04-vps" %}
