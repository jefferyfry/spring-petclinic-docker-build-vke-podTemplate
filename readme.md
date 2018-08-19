# Spring PetClinic Docker Build Pod Template for VMware Kubernetes Engine (VKE) Example

This is an example of running Docker builds on CloudBees Core on VMware Kubernetes Engine (VKE) using agent pod templates (instead of configuring in the Jenkins UI). See the podTemplate folder and the Jenkinsfile. This example runs a maven build, docker build, pushes the image to docker hub and deploys a new pod as a staging server. Check out this [video](https://www.youtube.com/watch?v=OLFMosFue5U&feature=youtu.be) for an overview. See the [Jenkinsfile](https://github.com/jefferyfry/spring-petclinic-docker-build-vke-podTemplate/blob/master/Jenkinsfile) for the pipeline.

<img width="920" alt="vke_pipeline" src="https://user-images.githubusercontent.com/6440106/44313011-84f72e80-a3b5-11e8-9a65-91b90de71687.png">

## Requirements
- CloudBees Core 1.212.2.1 with Kubernetes Plugin 1.10.2

