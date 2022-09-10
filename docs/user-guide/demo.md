---
layout: default
title: Demo
parent: User Guide
nav_order: 7
---

# Demos about how to use these actions
The [Demo project](https://github.com/jenkinsci/jfr-action-demo) can teach you how to use these actions.

## Language Demos

The following demos define the best practice of compiling your projects by using Jenkinsfile-runner Actions.
After going through these demos to get inspiration, you can choose the most suitable actions for your project.

* [Java demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/java): How to compile a Java Spring Boot project.
* [JavaScript demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/javascript/my-react-app): How to compile a React project.
* [Scala demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/scala/scalaexample): How to compile a Scala project.

## Advanced Usage

The following examples show how to configure your ephemeral Jenkins instances and plugins.
Each of them needs [JCasC](https://github.com/jenkinsci/configuration-as-code-plugin) to be set up.
If you want to debug new JCasC settings, testing in your local Jenkins controller is highly recommended.
After local testing, you can directly copy the related JCasC for your ephemeral Jenkins instance on the GitHub Actions runners.

* [Jenkins secrets demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/jenkins-secrets): How to use the secrets in the pipeline. It aims at using GitHub secrets to enhance security instead of using extra Jenkins plugins.
* [Jenkins environment variables demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/jenkins-envs): How to declare the environment variables in the GitHub workflow definitions and map them into the Jenkins pipelines.
* [s3-artifact-manager demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/s3-artifact-manager): How to store the pipeline results to an external storage by using s3-artifact-manager plugin.
* [Pipeline cache demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/jobcacher): How to reuse the dependencies downloaded before in such a FaaS context by uisng jobcacher plugin.
* [Remote agent demo](https://github.com/jenkinsci/jfr-action-demo/blob/master/demo/remote-agent): How to trigger the building in the remote agents. This example will be useful if you think the computation resources provided by GitHub is not enough or you need to compile the projects in different operating systems.
