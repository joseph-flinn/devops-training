# Sprint 5
One of the most important skills in the DevOps "field" is automation. The DevOps mindset is all
about exposing processes and the work that flows through them and then applying automation to break
through the bottlenecks that are identified in the flow of work. 

Most of this automation is done in Continuous Integration/Continuous Delivery (CI/CD) pipelines.
There are many different ci/cd pipeline softwares:

- Jenkins
- GitLab CI/CD
- GitHub Actions
- Azure DevOps Pipelines
- Drone.io

Every team and project is different and they need different things. This needs to be kept in mind 
when designing and building these pipelines. It is difficult to overhaul an entire system from 
front to back and deploy that into production. Most CTOs and VPs come into companies and draft a 
two year plan on how to revamp the infrastructure either with a new technology or a re-org or 
maybe both. These normally don't work and they get one and a half years into the process and aren't
anywhere closer to a goal. They quit and the process starts over again. All of that said, every team 
and project is different and will need different things out of their CI pipelines.

However, what we are shooting for is anything that will increase Deployment Frequency (while sustaining
and growing software stability), anything that will increase the speed of code getting into production,
anything that will help increase the time to recovery when your service/software goes down, and 
anything that will lead to less deployment failures. These are the four core metrics of Software
Delivery Performance which is directly correlated to the overall organizational performance. This is
what DevOps is all about.

Normally that looks like a build stage and a release stage. Sometimes you need other stages in there
for support (like a deploy stage when you need to separate your release and deploy work). This is what
we are going to be focusing on for the next few weeks.


## GitHub Actions
GitHub Actions is what we are going to use for this. It is built into GitHub so it is pretty
accessible. YAML is used for most of the configuration (like docker-compose files). The tasks are
mostly bash or some other shell language. GitHub Actions does also have other preconfigured tasks
that can used by passing in arguments (much like a function). These can be custom with technologies
like docker and nodejs.

GitHub Actions workflows are triggered with specific github events (opening a PR, closing a PR,
pushing a branch, creating a release, publishing a release, manual trigger, etc). Sprint


### Example
Let's look an example. We are going to choose a python library to look at because of the simplistic
pipeline model that they have (they don't need infrastructure created, they aren't being deployed
into a system that is then talking with a bunch of other microservices). 

I have recently created a simple build/release pipeline model for a project that I was using and that
looked like it could benefit from automation to maintain the releases on GitHub and publishing the 
package up to PyPI. It can be found [here](https://github.com/yfilali/graphql-pynamodb/pull/26/files)

#### Problem
When I first started using this project, the release version number on GitHub did not match the 
version number that was published on PyPI. This might not seem scary, but this could have been the 
outcome of a Supply Chain Attack. These are rare (we know of less than 10 that happen a year), but 
they can be devastating. In recent years they are becoming more a thing, or at least they are becoming
more a thing in the public spot light, such as the SolarWinds hack. The devastating thing about 
Supply Chain Attacks is that they don't only affect the organization that was hacked in the first place,
whoever is using that code library has also been compromised (version pinning is a must with 
dependencies). With over 98% of enterprise software using open source software, this is even more 
important to be aware of since essentially anyone can contribute to OSS and that might contain malware.
All that said, mismatched release versions is something that shouldn't happen and is scary if it is. 
Fortunately, I contacted the owner of the project and he mentioned that he had just forgot to cut the
release on GitHub when he published up to PyPI.

#### Solution
An automated process that releases the code on GitHub and then publishes it to PyPI. This process is
going to have two parts: 1) a build & release pipeline, and 2) a publish pipeline (aka, deploy).


##### Build
Whenever you are releasing and deploying software, it is essential to have build artifacts to release.
The artifacts really should be going through automated testing in the CI pipeline and have been 
prevalidated by a bunch of different processes that it is good to go. Only artifacts that have gone
through these processes should be released. These pipelines should not re-run specific build steps after
the pipeline (ie. Rebuild the code from scratch to deploy/release it). Everything should be released from
previous build artifacts.

A build pipeline normally does a few things: builds/compiles the code, tests the code and then packages
the code. The example that we are going through does the first and the last of these. This specific 
library requires a database to be run for testing and I didn't really have the time to figure out how
to set that up in GitHub Actions. However, there is a task stubbed in that is waiting for someone else
to come along and add it. We'll just assume that that is running its tests.

- Starting from the top, this workflow is triggered every time a branch is pushed up to GitHub. 
- Tests are are run as soon as possible to get feeback to the developer on whether or not their code is 
  going to break what is in production (or if it will be backwards compatible, or whatever else the tests 
  are testing for). 
- Any software that is being built and released needs a version number to track it. We pull this into 
  the build pipeline directory from the code
- We build/package the code to get it ready for release
- After packaging the code, we attach it to the build report to come back later if we need to for testing
  or for any other reason we would need to have a snapshot of those build artifacts (like pulling them
  to publish).
- I wanted to try out an idea of triggering releases automatically when the version number has been bumped
  and that version bump is merged into the default branch (main, master, dev, etc). So a version check is
  performed
- If the version has been bumped, create a draft release in GitHub with the new version and attach the 
  build artifacts to that release. The owner and/or maintainers of the project can then go and add release
  notes to the draft release and publish the release on GitHub (this tags the code which is like taking a
  snapshot in time)


##### Publish
After you build software, normally you would like to share it with people. Some software is strictly shared
on GitHub via its releases; however, that isn't the most user friendly way of installing software. Take
python libraries for example. We can't `pip install` a library from GitHub very easily. So, we need to 
deploy or publish that software so that everyone can use it.

- This workflow can be triggered in two different ways: by publishing a release (after adding release notes
  like in the last step of the biuld workflow list) or by manually running it. You never know what can 
  happen out in the wild. What if PyPIs network goes down and fails to publish your package but you already
  published your release? You can't go and push that button again. It is only a one time deal. So, having
  a way to manually trigger a deploy is a good idea. But really only if it will always deploy the same
  build artifact, no matter how often you run it.
- Get the release version from GitHub or from the manual trigger
- Use that version to pull the release assets (the build assets that were attached to the release).
- Push/publish/deploy those release assets
