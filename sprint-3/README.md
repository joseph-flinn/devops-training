# Sprint 3
This sprint is all about the cloud. The cloud is what really spurred the need for a DevOps 
mentality in the wider industry. As you read _The Pheonix Project_ you will see that the cloud
is not necessary for DevOps to exist. 

## SSH
---
While SSH isn't necessarily a cloud specific thing, it becomes very important when dealing 
with VMs. SSH is a program that allows remote terminal access to linux servers (and maybe 
windows? I don't know about that because I kind of refuse to deal with windows servers). Most
default SSH servers are configured to use the username and password that was created in 
setting up the OS. A simple connection command will look something like this:

```
ssh username@<ip-or-hostname-or-domain-name>
```

Now since the VMs that we are going to be using in the cloud are not going to be behind a 
firewall (like your home router's firewall), there is an extra step of security that we need
be aware of and that is creating an encryption certificate to use intead of a password. These 
use public/private keys (based on RSA or EC encryption) so that you are more protected against 
dictionary attacks against your password.

There are two tools that make this pretty easy: `ssh-keygen` and `ssh-copy-id`.

When using a certificate to log into a server via SSH:

```
ssh -i ~/.ssh/cert-name username@<ip-or-hostname-or-domain-name>
```

All of this being said, if you are using the `gcloud-sdk`, then you don't have to worry about 
this because it is handled for you. I would suggest connecting via `gcloud` and to review the
below documentation

- https://cloud.google.com/compute/docs/instances/connecting-to-instance#gcloud
- https://cloud.google.com/compute/docs/instances/ssh


### Hostname
The hostname of a machine is set manully on the server. To use via the host name, the client
computer must set the IP address to hostname conversion in its own `/etc/hosts` file (on 
linux; there's a hosts file somewhere in Windows, but I don't remember. An internet search 
could help here)

### Domain name
Mostly controlled by an external DNS server (Domain Name Server), domain names are used for 
people to more easily remember a web address versus having to remember a specific IP address.
DNS is also used because IP addresses can change and you don't want to have to send out 
messages to all of the users of your service that it can no longer be found at the old IP 
address and here's the new one. Just think if everyone had to remember the IP address for
`www.google.com` and had to update their brains to a new one every time it changed.

This updating is handled by the DNS servers. There are many many DNS servers in the world and
they normally all talk to eachother to or forward requests on to bigger ones. For instance, 
you can create a DNS server in your house, but it wouldn't have to store the IP addresses for
Google or Microsoft and then all of the little websites like Indaba and Spiceology. You can
set it up to forward requests for IPs that it doesn't have upstream to a larger DNS server 
(like Google's servers at 8.8.8.8 and 8.8.4.4).

Locally on a Linux machine, you can also override a DNS name in the `/etc/hosts` file as you
would a hostname

## Cloud Architecture
I'm not going to get too deep into Cloud Architecture since we only have about two weeks to
make you dangerous. Good cloud hyigene includes two main things: 1) well designed cloud 
networking with a well designed security perimeter and 2) solid user/group permissions (aka 
IAM or Identity and Access Management). There are other parts of cloud architecture that are 
important when it comes down to designing a specific solution, but these are the main two 
that are going to be important for every cloud account/organization/subscription.

I don't have a bunch of experience with designing an overall cloud network since most of the
accounts that I have worked in have been set up by someone else. But understanding the basic
networking between subnets and understanding that cloud networks should have only a couple of
entry points versus every VM being exposed to the internet (like we will be doing for speed
and efficiency through this journey). This is something that you will have to learn either by
experimentation or seeing someone's cloud network that has been properly set up. There might
be some good articles on the internet for this.

IAM consists of creating/inviting users to join your cloud and then either granting them 
granular permissions for what they can do for each service or you can create a group with 
those granular permissions and add the user to the group that is appropriate. For instance, we
could create a group named Ops and a group named Devs. Devs would need access for things like
VM access or read/write access to storage buckets where maybe only Ops has access to 
create/destroy VMs and storage (this is just an example. I don't recommend restricting Dev
permissions on things like creating/destroying VMs in a Testing account, but it would probably
makes sense in a Production account). You can get a feel for this by adding me to your GCP 
account and granting me certain permissions and the like. 

## Cloud Services
---
There are many many many different cloud service on all of the different clouds. There are 
services that host AI applications for you. There are services that manage databases for you 
so you don't have to worry about OS patching or indexing. All you have to do is dump your 
organized data there and it will take care of the rest.

One thing to note on all of the different services on the different clouds, cloud vendor lock
in is something that should be considered when choosing the technologies that are used. For
instance, AWS has a technology called CloudFormation that is their own in house Infrastructure
as Code. You can write code that describes/builds infrastructure and that code can then be 
version controlled so that you know exactly what you have (or should have) at all times. 
However, CloudFormation can only be used on AWS and does not work for any other clouds. 
Terraform is also an IaC technology, but it works on many different clouds. Using Terraform 
allows you to only have to learn one technology to get the benefits of IaC versus being stuck
to one cloud provider. Every company that I have worked for utilizes more than one cloud. 
