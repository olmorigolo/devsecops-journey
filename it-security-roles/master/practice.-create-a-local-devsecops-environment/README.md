---
description: A practical guide on building a pipeline with security tools
---

# Building a DevSecOps CD/CI Pipeline

## Step #1 Get A Vulnerable Web App

Get one of the vulnerable web apps from the Vulnerable Web Apps Page on your machine.

{% content-ref url="../../web-application-security-testing/self-hosted-training-lab/vulnerable-web-apps.md" %}
[vulnerable-web-apps.md](../../web-application-security-testing/self-hosted-training-lab/vulnerable-web-apps.md)
{% endcontent-ref %}

## Step #2 Get a CD/CI build server

Choose one that fits you most. Most common gitlab, travis, github actions, CircleCI, TeamCity, Bamboo

{% content-ref url="get-a-devops-platform.md" %}
[get-a-devops-platform.md](get-a-devops-platform.md)
{% endcontent-ref %}

## Step #3 Create your build pipeline

How to integrate a tool into my build pipeline? Go through the different types of scanners and find a tool for each scan type that fits your code/web app environment. Then try it out on you local machine against your app under test.

OWASP describes a devsecops maturity model that one can follow in order to implement every stage of a complete devsecops pipeline. One step at a time starting at maturity level 1.&#x20;

{% embed url="https://owasp.org/www-project-devsecops-maturity-model/" %}

### Stages of the pipeline

{% content-ref url="software-component-analysis-sca.md" %}
[software-component-analysis-sca.md](software-component-analysis-sca.md)
{% endcontent-ref %}

{% content-ref url="static-application-security-testing-sast.md" %}
[static-application-security-testing-sast.md](static-application-security-testing-sast.md)
{% endcontent-ref %}

{% content-ref url="dynamic-application-security-testing-dast.md" %}
[dynamic-application-security-testing-dast.md](dynamic-application-security-testing-dast.md)
{% endcontent-ref %}

{% content-ref url="system-hardening.md" %}
[system-hardening.md](system-hardening.md)
{% endcontent-ref %}

{% content-ref url="system-compliance-analysis.md" %}
[system-compliance-analysis.md](system-compliance-analysis.md)
{% endcontent-ref %}

{% content-ref url="vulnerability-analysis.md" %}
[vulnerability-analysis.md](vulnerability-analysis.md)
{% endcontent-ref %}
