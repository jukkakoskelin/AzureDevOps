# Quick cheat sheet for select between different pipeline types

Dated: 20.05.2020. Some of this will be obsolete as more [Azure DevOps features ship](https://docs.microsoft.com/en-us/azure/devops/release-notes/features-timeline). Some might be obsolete already.

Have a peek of [key concepts](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/key-pipelines-concepts?view=azure-devops), first. 

## Classic build definitions vs YAML-based build pipelines

No real reason to choose classic build definitions (aka. build definitions made with visual editor), except if you already have a ton of them. Classic build definitions are good for learning how pipelines work, but that's about it.

## Classic release defintions (and classic or yaml based builds) vs multi-stage pipelines

* If you're on Azure DevOps Server 2019, you don't have the luxury of using multi-stage -pipelines, so this is easy for you.
* Deploying on-premises: go with classic release definitions, as you'll need Deployment Groups. They are only available for release defintions.
* Deploying to virtual machines in Azure or Azure Kubernetes Service: go with multi-stage pipelines as they support Environments. Environments give you visibility to what you have deployed (to that resource), approvals and other security controls. Environments are kind of synonym for Deployment groups (for Virtual Machines, both deploy a local agent to target machines), but a lot more useful for tracking your deployments.
* Deploying something to Azure via service endpoints: up to you, as you can run your jobs with hosted agents.
* Separation of build and deployment: release definitions have a much clearer concept when it comes to this. Your build creates a build artifact and release is created if certain conditions (in continuous deployment trigger) are met. With multi-stage pipelines and the new concept of pipeline artifacts, this separation is non-existent (event if you could kind of build one).
* Flow of deployment: again, the capabilities are similar, but creating release defintions is a visual experience and you can see how your flow branches based on your stage triggers and artifact filters. With multi-stage pipelines you're conceptually more drawn into a straight pipeline. Whether this is good or not is up to you.
* If in doubt, always go YAML.

## Stages vs Environments

A subcategory of defintions vs yaml. Classic release definitions have a concept of Stage, which is a logical unit of deployment; it's really just a container for agent jobs and tasks. Well, a container with triggers with approval controls. One of the downside for stages is that (at least for people coming from traditional pet-type of environments that you don't ever tear down) they usually end up representing your environments (dev/test/qa/prod/etc). If you end up with multiple release definitions, you usually end up having similar stages in each of them. So, 10 pipelines and 10 qa-stages. Problem is, that nothing really ties these "environments" is different pipelines together, and there is no easy way to keep track of what you have deployed to where.

Environments are the answer to this, if you are able to make use of them. They _are_ representation of your environments, or a collection of resources that make up your environments.

