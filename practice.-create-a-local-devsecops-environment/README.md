---
description: A practical guide on building a pipeline with security tools
---

# Building a DevSecOps CD/CI Pipeline

## Get A Vulnerable Web App

Get one of the vulnerable web apps from the Vulnerable Web Apps Page on your machine.

{% page-ref page="vulnerable-web-apps.md" %}

## Get a CD/CI build server

Choose one that fits you most. Most common gitlab, travis, github actions, CircleCI, TeamCity, Bamboo

### Jenkins

Download and set-up a local jenkins with docker.

{% embed url="https://www.jenkins.io/doc/book/installing/docker/" %}

### Gitlab

Install your own gitlab server on a virtual machine as described in [https://embeddedinventor.com/complete-guide-to-setting-up-gitlab-locally-on-mac/\#STEP4\_Create\_and\_edit\_gitlab\_ciyml\_to\_run\_the\_above\_script](https://embeddedinventor.com/complete-guide-to-setting-up-gitlab-locally-on-mac/#STEP4_Create_and_edit_gitlab_ciyml_to_run_the_above_script)

Register your local machine as a runner and run it with `gitlab-runner --debug run` 

to see if it connects to the server

## Create your build pipeline

How to integrate a tool into my build pipeline? Go through the different types of scanners and find a tool for each scan type that fits your code/web app environment. Then try it out on you local machine against your app under test.

OWASP describes a devsecops maturity model that one can follow in order to implement every stage of a complete devsecops pipeline. One step at a time starting at maturity level 1. 

{% embed url="https://owasp.org/www-project-devsecops-maturity-model/" %}

### Stages of the pipeline

{% page-ref page="software-component-analysis-sca.md" %}

{% page-ref page="static-application-security-testing-sast.md" %}

{% page-ref page="dynamic-application-security-testing-dast.md" %}





