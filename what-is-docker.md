# What is Docker

Docker is a container technology : A tool for creating and managing containers.

Container : A standardized unit of software. A package of code and all the dependencies required to run that code.&#x20;

Solves the 'But it runs on my computer' problem.



#### Containers vs. VM (Virtual Machines)

According to the creators of Docker, containers shouldn't be compared to VMs, because the processes run on the host VM process list. They are just chroot on steroids.

**Virtual machine** creates multiple instances of a complete operating system, each running on its own virtualized hardware. Which means, VMs have their own independent kernel, memory, disk spaces and other resources. They emulate the underlying hardware.

Each virtual machine has its own set of dedicated resource including CPU, memory, storage and network interfaces allocated and managed by the hypervisor. The resources aren't shared with other VMs on the same host.

**Containers** are isolated instances that share the host system's kernel but have their own file systems, processes, and network interfaces. So, containers do not require hardware emulation. They leverage the host OS's kernel and share it among multiple containers. Containers virtualize the OS resource such as file system, network interface, process trees, rather than virtualizing the hardware itself.

Containers share resources like CPU and memory but has isolation for file system, networking and process namespace so they don't interfere with each other.

> Containers run apps natively on the host machine’s kernel. They have better performance characteristics than virtual machines that only get virtual access to host resources through a hypervisor. Containers can get native access, each one running in a discrete process, taking no more memory than any other executable.

So, a question might arise, Docker isolates containers and facilitates sharing of resource through kernel namespaces and cgroups, these features are native to linux. Also, A container does not have a full operating system like a traditional virtual machine (VM) does. Instead, a container shares the kernel of the host operating system, which allows it to be lightweight and efficient. So, what happens in case of Windows, where the kernels are entirely different when the container has a linux filesystem ?

Answer to this question is, when Docker is run in a Windows environment, it uses a different approach compared to running Docker on a Linux system. In Windows, Docker relies on a lightweight virtual machine (VM) called "MobyLinuxVM" to provide the necessary Linux kernel capabilities for running Linux-based containers.&#x20;



{% embed url="https://youtu.be/sK5i-N34im8?list=PLBmVKD7o3L8v7Kl_XXh3KaJl9Qw2lyuFl" %}





{% embed url="https://www.ibm.com/cloud/blog/containers-vs-vms" %}

{% embed url="https://bikramat.medium.com/docker-architecture-and-its-component-b7e8cc172b55" %}

