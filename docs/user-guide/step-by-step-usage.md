---
layout: default
title: Step by step usage
parent: User Guide
nav_order: 4
---

# Step by step usage
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Container actions usage
1. Prepare a Jenkinsfile in your repository. You can check [the basic syntax of Jenkins pipeline definition](https://www.jenkins.io/doc/book/pipeline/syntax/).
2. Prepare a workflow definition under the `.github/workflows` directory. You can check [the official manual](https://docs.github.com/en/actions) for more details.
3. In your GitHub Action workflow definition, you need to follow these steps when calling other actions in sequence:
   1. Use a ubuntu runner for the job.
   ```yaml
    jobs:
        job-name:
          runs-on: ubuntu-latest   
   ```
   2. If you use jfr-container-action, you need to declare using the `ghcr.io/jenkinsci/jenkinsfile-runner:master` or any image extended it. If you use jfr-static-image-action, you can skip this step.
   ```yaml
    jobs:
        job-name:
          runs-on: ubuntu-latest
          container:
            image: ghcr.io/jenkinsci/jenkinsfile-runner:master             
   ```   
   3. Call the `actions/checkout@v2` to pull your codes into the runner. "Call" means `uses` in the workflow definition specifically. You can check [the details about "uses" keyword](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsuses).
   ```yaml
   - uses: actions/checkout@v2
   ```
   4. Call the Jenkinsfile-runner actions.
      1. If you use jfr-container-action, you need to call `jenkinsci/jfr-container-action@master` and give necessary inputs.
      ```yaml
      uses:
        jenkinsci/jfr-container-action@master
      with:
        command: run
        jenkinsfile: Jenkinsfile
        pluginstxt: plugins.txt
        jcasc: jcasc.yml      
      ```
      2.  If you use jfr-static-image-action, you need to call `jenkinsci/jfr-static-image-action@master` and give necessary inputs. See the [examples](#example-workflows) for these two actions.
      ```yaml
      uses:
        jenkinsci/jfr-container-action@master
      with:
        command: run
        jenkinsfile: Jenkinsfile
        pluginstxt: plugins.txt
        jcasc: jcasc.yml      
      ```

## Runtime actions usage
1. Prepare a Jenkinsfile in your repository.
2. Prepare a workflow definition under the `.github/workflows` directory.
3. In your GitHub Action workflow definition, you need to follow these steps when calling other actions in sequence:
   1. Use the runners you prefer. You can choose Linux, macOS or Windows.
   ```yaml
    jobs:
        job-name:
          runs-on: ubuntu-latest   
   ```   
   2. Call the `actions/checkout@v2` to pull your codes into the runner.
   ```yaml
    jobs:
        job-name:
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v2           
   ```      
   1. Set up the Jenkins environment by using `jenkinsci/jfr-setup-action@master`.
   ```yaml
    jobs:
        job-name:
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v2
            - uese: jenkinsci/jfr-setup-action@master           
   ```    
   1. Install extra plugins by using `jenkinsci/jfr-plugin-installation-action@master`. This step is optional.
   ```yaml
    jobs:
        job-name:
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v2
            - uese: jenkinsci/jfr-setup-action@master
            - uses: jenkinsci/jfr-plugin-installation-action@master
              with:
                pluginstxt: plugins.txt                     
   ```   
   1. Run the Jenkins pipeline by using `jenkinsci/jfr-runtime-action@master`.
   ```yaml
    jobs:
        job-name:
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v2
            - uese: jenkinsci/jfr-setup-action@master
            - uses: jenkinsci/jfr-plugin-installation-action@master
              with:
                pluginstxt: plugins.txt
            - uses: jenkinsci/jfr-runtime-action@master
              with:
                command: run
                jenkinsfile: Jenkinsfile
                jcasc: jcasc.yml                                   
   ```    