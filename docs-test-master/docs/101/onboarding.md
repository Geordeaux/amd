---
id: onboarding
title: New Joiners Onboarding
---

## Welcome Aboard !

This doc is for Web Infra team's new joiner to quickly setup dev environment and get necessary permissions.

Please help keep this doc up to date with latest information.


## Dev Environment setup
Install following tools and packages.
- IDE of your choice (such as [VS Code](https://code.visualstudio.com/download), [IntelliJ](https://www.jetbrains.com/idea/download))
- brew (the MacOS package manager)
- git
- aws-cli
- saml2aws
- [bazel](https://docs.bazel.build/versions/master/install.html)
- kubectl
- helm
- wasme
- [go](https://golang.org/dl/) (if you are Golang devloper)
- [krew](https://krew.sigs.k8s.io/docs/user-guide/setup/install/)

## Apps

### [GitHub Enterprise](https://git.toolsfdg.net/)
#### Setup
1. Search `GitHub Enterprise` on [Okta](https://binance.okta.com/app/UserHome) and request access.
2. Once access request is approved, login to GHE at least once through the okta dashboard (the GHE account will be created when you log in for the first time). 
3. Request accesss to our team's [Git repo](https://git.toolsfdg.net/mono/mono) from our team's DevOps engineer.
4. Our GHE only allows HTTPS to pull the code repository. So follow this [Personal Access token setup](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=19366568) tutorial. 

#### Code Submission
Our GHE does not allow direct write to upstream [git repo](https://git.toolsfdg.net/mono/mono). So follow this [code submission guide](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=4423977).

### [AWS](https://console.aws.amazon.com/console/home?#) (if required)
1. Determine clusters (e.g. Dev, Tools, Prod) and the level of access needed. 
2. Search `AWS Binance` on [Okta](https://binance.okta.com/app/UserHome) and request access.
3. Request [DevOps repo](https://git.toolsfdg.net/devops/terraform-binance-aws-iam) access from our team's DevOps engineer.
4. Create a similar PR like [this](https://git.toolsfdg.net/devops/terraform-binance-aws-iam/commit/c62cf082882b7bcfaa3cd49894cbf0b284bfd9c6) and get it merged.
5. After PR is merged and the Terraform change is applied, you should be able to access AWS console from Okta.
6. Follow [this wiki](https://confluence.toolsfdg.net/display/Technology/Request+for+IAM+permission) for additional IAM permissions and then reach out to `Abel Xie`.
7. Follow [this wiki](http://book.toolsfdg.net/intro/how_to_access_aws_resource.html) to access AWS ressources from your laptop.
8. Here is another useful [wiki](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=42446191) that explains AWS IAM 101.


## Mono repo directory structure
Please go throught [this](mono/directory-structures) doc to understand our [Mono repo's](https://git.toolsfdg.net/mono/mono) directory structure.


## Important Points
- Docker image tag is same as the git commit hash. e.g. https://git.toolsfdg.net/mono/mono/commit/dfd44c85bca47fef81efccb89a31a85ab5b15cb7 commit will generate docker with tag `dfd44c85bca47fef81efccb89a31a85ab5b15cb7`.

## Internal Links
- [Web Infra Team Roles & Responsibilities](https://docs.google.com/presentation/d/1ZGw1SP4nGmoA7ky9orskOxlyJnYvvxurFWZBcKPA2qE/edit#slide=id.g8afe39d7e8_0_0)
- [Web Infra Team Progress doc](https://docs.google.com/document/d/1dCQAvjknB5NKZjnsyFpESX4IsrV8U5eIr_KHVp7qKbg/edit#heading=h.90jbv131zuuc)
- [Prow](https://prow.toolsfdg.net/) - prow jobs.
- [Tekton](https://tekton.toolsfdg.net/) - CI pipelines.
- [Grafana](https://monitor-prod.toolsfdg.net/) - dashboard for monitoring systems.
- [Kiali](https://kiali.prod-com.toolsfdg.net/) - service mesh management console.
- [Jaeger](http://tracing.prod-com.toolsfdg.net/jaeger/search) - distributed tracing.

## FE bootcamp trainings
Please find all FE bootcamp training recordings [here](https://docs.google.com/spreadsheets/d/1pihWO6B7aLzBSFNz_gkBcFV6Y49j2QWsePIqg7A6kgE/edit#gid=1161341563)

## Reading Material
- [Kubernetes test-infra](https://github.com/kubernetes/test-infra)
- [Prow Jobs](https://github.com/kubernetes/test-infra/blob/master/prow/jobs.md)
- [Helm](https://helm.sh/). Here is a useful YouTube [video](https://www.youtube.com/watch?v=-ykwb1d0DXU).
- [Istio architecture](https://istio.io/latest/docs/ops/deployment/architecture/)
- [Envoy](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy)
- [Kiali](https://kiali.io/)
- [YAML tutorial](https://learnxinyminutes.com/docs/yaml/)
- [Docusaurus](https://docusaurus.io/)
