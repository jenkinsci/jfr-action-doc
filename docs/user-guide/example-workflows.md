---
layout: default
title: Example workflows
parent: User Guide
nav_order: 5
---

# Example workflows
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

There are three common cases about how to play with these actions. Although the user interfaces are similar to each other, there are still some subtle differences. The runtime actions are deprecated now. The users can use the [jfr-container-action](#container-job-action) and [jfr-static-image-action](#docker-container-action).

## Container job action
This case is realized by jfr-container-action. If the job uses this action, it will run the Jenkins pipeline and other GitHub Actions in the prebuilt container provided by [ghcr.io/jenkinsci/jenkinsfile-runner:master](https://github.com/jenkinsci/jenkinsfile-runner/pkgs/container/jenkinsfile-runner). The **extra prerequisite** of this action is that you need to declare the image usage of ghcr.io/jenkinsci/jenkinsfile-runner:master at the start of the job.
```yaml
name: Java CI
on: [push]
jobs:
  jenkins-container-pipeline:
    runs-on: ubuntu-latest
    name: jenkins-prebuilt-container-test
    container:
      # prerequisite
      image: ghcr.io/jenkinsci/jenkinsfile-runner:master
    steps:
      - uses: actions/checkout@v2
      # jfr-container-action
      - name: Jenkins pipeline in the container
        id: jenkins_pipeline_container
        uses:
          jenkinsci/jfr-container-action@master
        with:
          command: run
          jenkinsfile: Jenkinsfile
          pluginstxt: plugins.txt
          jcasc: jcasc.yml
```
Some users might want to configure the container environment. The recommendation is that you should extend the [ghcr.io/jenkinsci/jenkinsfile-runner:master](https://github.com/jenkinsci/jenkinsfile-runner/pkgs/container/jenkinsfile-runner) vanilla image and then you need to build and push it to your own registry. Finally, you can replace the vanilla image with your own custimized image. The invocation of jfr-container-action can be done in a similar way.
```yaml
name: Java CI
on: [push]
jobs:
  jenkins-container-pipeline:
    runs-on: ubuntu-latest
    name: jenkins-prebuilt-container-test
    container:
      # prerequisite: extendance of ghcr.io/jenkinsci/jenkinsfile-runner:master
      image: path/to/your_own_image
    steps:
      - uses: actions/checkout@v2
      # jfr-container-action
      - name: Jenkins pipeline in the container
        id: jenkins_pipeline_container
        uses:
          jenkinsci/jfr-container-action@master
        with:
          command: run
          jenkinsfile: Jenkinsfile
          pluginstxt: plugins.txt
          jcasc: jcasc.yml
```

## Docker container action
This case is realized by jfr-static-image-action. This action has its own working environment. It won't have extra environment relationship with the on demand VM outside unless the user mounts other directories to the container (For example, checkout action if exists). After the docker action ends, this container will be deleted. The users may check the introduction of [Docker container action](https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action#introduction) before using this action.
```yaml
name: Java CI
on: [push]
jobs:
  jenkins-static-image-pipeline:
    runs-on: ubuntu-latest
    name: jenkins-static-image-pipeline-test
    steps:
      - uses: actions/checkout@v2
      # jfr-static-image-action
      - name: Jenkins pipeline with the static image
        id: jenkins_pipeline_image
        uses:
          jenkinsci/jfr-static-image-action@master
        with:
          command: run
          jenkinsfile: Jenkinsfile
          pluginstxt: plugins.txt
          jcasc: jcasc.yml
```

## Runtime action
This case is realized by the combination of jfr-setup-action, jfr-plugin-installation-action and jfr-runtime-action. It will download all the dependencies and run the pipeline at the host machine directly. Its advantage is that it can support Linux, macOS and Windows runners. Its main disadvantage is the possibility of suffering from a plugins.jenkins.io outage.
```yaml
name: Java CI
on: [push]
jobs:
  jenkins-runtime-pipeline:
    # Run all the actions in the on demand VM.
    needs: syntax-check
    strategy:
      matrix:
        os: [ubuntu-latest, macOS-latest, windows-latest]    
    runs-on: ${{ matrix.os }}
    name: jenkins-runtime-pipeline-test
    steps:
      - uses: actions/checkout@v2
      - name : Setup Jenkins
        uses: jenkinsci/jfr-setup-action@master
      - name: Jenkins plugins download
        id: jenkins_plugins_download
        uses: jenkinsci/jfr-plugin-installation-action@master
        with:
          pluginstxt: plugins.txt
      - name: Run Jenkins pipeline
        id: run_jenkins_pipeline
        uses: jenkinsci/jfr-runtime-action@master
        with:
          command: run
          jenkinsfile: Jenkinsfile
          jcasc: jcasc.yml
```