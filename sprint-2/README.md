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


## Docker
As important as Docker is, I personally wouldn't get too attached to it. The future of it
is a bit unknown as it was just acquired by a company. Open source projects being acquired
by companies is a bit scary. This is a bit of a bias, but the best open source projects are
the ones that are "acquired" by the Linux Foundation. The fact that Docker wasn't picked up
by them shows me that it isn't as important in the future of the industry as most might
think. `containerd` is the containerization technology that the Linux Foundation maintains.
All of this being said, we are going to choose Docker as our containerization tech because
that is what I am most familiar with and I haven't yet done too much research or 
experimentation with `containerd` or any of the other containerization tech (`lxc/lxd`, 
`rkt`, `runc` (what Docker is built off of)). K8s just changed the default container 
runtime from Docker to `containerd`.

There are some ideas and how they relate to eachother to be aware of as you dive into the 
exciting and possibly daunting world of Docker:
- container
- image
- `docker build`
- dockerfile
- `docker run`
- docker-compose
- docker-compose.yaml

### Container
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

### Image
This is an entity/file/binary that is prebuilt that is needed to run a container. The 
container runtime goes and gets the specified image and loads a container from it. Other
images are used to build every other image (now that I think about it, I'm not really 
sure how the base images are built...). The base images of most (but probably all) containers
are blank OS containers including the most popular: `Ubuntu`, `Debian`, and `Alpine`. Alpine
Linux is a very barebones operating system that has nothing that it doesn't need. These are
very good for containers in production. It limits the attack surfaces avaiable to attackers
and is about 10x smaller in size, using less system resources.

### `docker build`
`docker build` is the command that we will be using to build images from `Dockerfile`s.

### Dockerfile
The Dockerfile is the file that describes the image to be built. It includes all of the
commands that need to be run to build the image. 

### `docker run`
`docker run` is one of the commands that we will be using to run containrs from images.

### docker-compose
`docker-compose` is a tool that allows us to describe relationships between containers and
run them together. It is the first step into the world of container orchestration (and this 
will be the only step we are taking together). These files are normally used for development 
purposes only. They can also be used to deploy to Docker Swarm clusters, however these are 
not really suited for production. There is much better container orchestration technology for 
production. These are a bit out of scope for this intro to DevOps. After Sprint 6, we can 
definitely chat about these and see if we can't get you spun up on some or at least headed in 
the right direction. 

### docker-compose.yaml
Like a Dockerfile is a description of a docker image, a docker-compose.yaml file describes the
relationships between containers. It also automates/describes the `docker run` commands for us 
so that we don't have to write a bunch of massive `docker run` statements one after another. On
top of that, it gives us an easy way to track our work in `git` for our future selves or others
that we are working with. 


# Conclusion
Linux is one of the more important technologies in the DevOps area. Most things are built on top
of it, so having a good understanding of it and how it works is paramount to being successful.
However, this will take time and experimentation which is why we are starting with it. There 
won't be much more discussion on specifics of Linux in the coming Sprints, but there may be a
comment here or there. Getting comfortable with the terminal and all of the power that it provides
is very important. Windows and Mac are catering more towards usability where Linux is like bowling
without the bumpers. There will be no warning if you want to completely destroy your OS with
`sudo rm -rf /` ([don't drink and root](https://hacker-gadgets.com/wp-content/uploads/2019/11/13450-2d8a4f.jpg)).

Containerization has enabled what the industry knows as the field of DevOps (one of the most 
misunderstood terms in industry...) for more on this). As you read
The Pheonix Project, you will see what industry practices were throughout all of industry and what
they continue to be at some companies. Deploying to production was a pain. The environments that
developers were using (their laptops) did not match what was being used in production. Just think
of the caos of developing in Windows and then deploying that code to Linux servers. But it isn't
the developers that do the deploying. The code is handed off to Ops to figure out how to build
and run it in production. Ops doesn't really know how it works because they didn't build it. 
Containers try to step in and solve part of this problem, the problem of "i don't know why it
isn't running in prod. It runs on my machine ¯\_(ツ)_/¯".

DevOps is one of the most misunderstood terms in industry. Companies look around and see other
companies succeeding at break neck speeds and they do case studies around them. "These comapnies
are doing DevOps. What does that even mean? Who knows. Let's hire an engineer to figure out what 
to do". There companies are too focused on the technical side of things instead of the process 
side of things. Taking DevOps technical practices and overlaying them on an organization that
isn't following DevOps philisophical processes can only be met with minimal success. Automation
is very important, but if there aren't process changes at the organizationaly level, the requests
and expectation of the automation is going to start becoming impossible to reach. If a company 
wants painless deployments to production every single time, they need to be willing to change how
they expect things to deploy to production. Deploys need to be automated to be done with every
small change, not every month or every couple of months. Introducing that many changes at once
will hide any problems that might occur. If you want something to be painless, you have to do it
more and more to iron out all of the issues and continuing to iron them out. Most of the industry
sees DevOps as what DevOps practioners know as automation techniques. But DevOps is much more than
automation. It is the breaking down of walls between developers and operations. It is about fostering
collaboration between the two teams, leading to a more stable and agile organization that can 
quickly outpace the competition. 
