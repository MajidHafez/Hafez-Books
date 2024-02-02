## What is the container?
There is a process on Linux that is isolated and does not see other processes and its file system is limited and a special directory is `chrooted` for it. And these processes are not connected with other processes by default and are isolated 
> (and these processes can be connected together and we call them a container)

The concept of container is not a new concept and we had a concept called `BSD Jail` in the `namespace` discussion in Linux and also in `BSD`.
It turns out that a `BSD` was divided into smaller BSDs, and the processes and resources of each BSD were separated from other BSDs, but they were actually on the same big BSD.

Or in Linux we have the same concept under the title of `chroot`.
So, in fact, `container managers` do not do anything strange by themselves and only use the features of the Linux kernel to containerize processes.
Almost until kernel 2.6, `BSD` and `Linux` could be compared, but after that, the Linux kernel, with the features it provided quickly and features suitable for the market daemon, practically left BSD behind!

Little by little, the issue of isolating processes became very important and needed. There was a time when it was more necessary to isolate only the file system of a process, for example, we used to say that a process is only `/` chrooted so that it cannot see the entire file system, but gradually this It expanded to other topics, for example, they wanted a process to not be able to see the list of other processes in general.

Or in the discussion of `Networking`, even though the processes were isolated, they used a `network stack`, which means that all the traffic of all the isolated processes was exchanged from a network card.
The concept of `Namespaces` in Linux is a broad concept, a large part of which was contributed by google... a part of it was contributed by redhat and... until we reached the concept of namespace today.
In fact, Namespace allows you to run a process in an environment where almost everything is isolated.

## Question> What can namespace isolate?

### Process ID (pid)
We have namespace which can isolate process id trees

### Mount (mnt)
Or the process in a namespace can not see other mount points

### Network
A namespace can have a separate network stack (a feature that made many big companies interested in using the Linux kernel), which means that each process can have a separate virtual network card, a separate virtual routing table, a separate iptables, a separate firewall, etc.

### User ID
A namespace can have separate users for each process.

### Inter-Process Communication (IPC)
Isolates inter-process communication resources such as System V IPC (Inter-Process Communication) and POSIX message queues. Processes in different IPC namespaces cannot communicate through these mechanisms.

### UTS namespace (uts)
Isolates hostname and NIS domain name, allowing processes inside the namespace to have their own unique values for these identifiers.

### Control Group (Cgroup)
Each process can be assigned specific hardware resources

### Time namespaces
Each process can have its own time

In the future, things like syslog namespaces are going to be added.

> The concept of container exists only on the Linux kernel, so how do we use the container on Windows or Mac?

There is a product called `docker desktop` (now or other container managers other than docker may have examples of this product) which runs a Linux vm in its background to run on Windows or Mac. , so practically we have the container only on the Linux kernel.

*Note*: When we talk about virtualization, a hypervisor layer comes and abstracts the host hardware, for example, into virtual memory, virtual processor, etc., but the container is a process in Linux that runs directly on the Linux kernel itself, and only this process isolated.
Or when we talk about venv in Python, it has nothing to do with the container... venv actually changes the path related to the executable of that python3 for each venv. And this way we can have several Pythons installed on our system.