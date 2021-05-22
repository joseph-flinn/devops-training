# Sprint 4
When dealing with the cloud and all of the different options that there are to build somthing, it is
hard to remember what everything is and how it has been set up, especially months after the initial
setup. What happens if you need to create another VM in a cluster? What if you have to replicate
your entire US based cloud in the EU? 

Terraform is a technology in the class of Infrastructure as Code (IaC). IaC is used to store
all of the settings that were used in the creation of all of the cloud infrastructure. It is an easy
way to track and replicate cloud infrastructure. I have personally found it easier to build the
cloud via terraform than to go in and click the buttons. 

Terraform uses a description laguage called HCL. it is similar to JSON and YAML, but it is specific
to HashiCorp products (Terraform, Nomad, Consul, Vault, etc). 

We are going to work through a tutorial and then recreate your VM that you have been using for
docker.
