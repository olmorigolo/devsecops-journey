---
description: A practical guide on building a pipeline with security tools
---

# Building a DevSecOps CD/CI Pipeline

## Step \#1 Get A Vulnerable Web App

Get one of the vulnerable web apps from the Vulnerable Web Apps Page on your machine.

{% page-ref page="../self-hosted-training-lab/vulnerable-web-apps.md" %}

## Step \#2 Get a CD/CI build server

Choose one that fits you most. Most common gitlab, travis, github actions, CircleCI, TeamCity, Bamboo

{% page-ref page="get-a-devops-platform.md" %}

## Step \#3 Create your build pipeline

How to integrate a tool into my build pipeline? Go through the different types of scanners and find a tool for each scan type that fits your code/web app environment. Then try it out on you local machine against your app under test.

OWASP describes a devsecops maturity model that one can follow in order to implement every stage of a complete devsecops pipeline. One step at a time starting at maturity level 1. 

{% embed url="https://owasp.org/www-project-devsecops-maturity-model/" %}

### Stages of the pipeline

{% page-ref page="software-component-analysis-sca.md" %}

{% page-ref page="static-application-security-testing-sast.md" %}

{% page-ref page="dynamic-application-security-testing-dast.md" %}

{% page-ref page="system-hardening.md" %}

{% page-ref page="system-compliance-analysis.md" %}

{% page-ref page="vulnerability-analysis.md" %}

