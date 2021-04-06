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


## GitHub Actions
GitHub Actions is what we are going to use for this. It is built into GitHub so it is pretty
accessible. YAML is used for most of the configuration (like docker-compose files). The tasks are
mostly bash or some other shell language. GitHub Actions does also have other preconfigured tasks
that can used by passing in arguments (much like a function). These can be custom with technologies
like docker and nodejs.

GitHub Actions workflows are triggered with specific github events (opening a PR, closing a PR,
pushing a branch, creating a release, publishing a release, manual trigger, etc). Sprint
