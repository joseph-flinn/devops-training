# Sprint 2

This sprint, we will be focusing in on more Linux experience which, in my experience, 
organically grows as you grapple with getting specific things working or other tools setup.
On top of the ever expanding experience with Linux, we will be diving into the exciting
world of containers; Docker specifically.

Most likely, this sprint will be tough; possibly the most tough of all of them. If I had to
one that is the most important, this one would be it. Everything else diverges from this one.

## Linux
Some things of note with Linux: everything is a file in Linux. Every file has
file permissions for `user` (u), `group` (g), and `root` (r). The set of permissions are 
`read` (4), `write` (2), and `execute` (1). If a file has a permission of 700 
(4+2+1)u (0+0+0)g (0+0+0)r, only the user that created the file can view it, no one else.

Common file commands:
`ls -l` will list all of the files in a directory along with the permissions
`chmod` will change the file permissions of a specified file or directory using the 3 digits
`chown` will change the ownership of files with both <user>:<group>

## Containers
Containers are similar to Virtual Machines, but their virtualization is a bit higher up in 
the stack. They utilize something in the kernel called `control groups (cgroups)` that 
"limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc).
Essentially, this means that each container has it's own process space, network stack, and 
memory space seperate from the other containers and the other processes in the system.

```
-----------------------------------
|   App    |    VM    | Container |
|---------------------------------|
|    OS    |    VM    | Container |
|---------------------------------|
|  Kernel  |    VM    |           |
|---------------------------------|
|    HAL   |    VM    |           |
|---------------------------------|
| Hardware |          |           |
-----------------------------------
```

Docker is currently the most popluar container technology, but it is moving towards a bit 
more standardized abstrction with `containerd`. Kubernetes just moved away from docker as 
its main conatiner system to containerd. 
