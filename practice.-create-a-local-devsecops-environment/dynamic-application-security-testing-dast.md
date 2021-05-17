---
description: Maturity Model Level 3+4
---

# Dynamic Application Security Testing \(DAST\)

## What is it?

DAST tools analyse the running application, the code at run time. It takes much time. DAST tools are most effective when a lot of spidering has been done on the application under test. Thus the more spidering, the better results. But spidering is a process thta takes time! DAST tools can be run on 

* the application \(i.e. with the zap proxy tool\)
* configuration
* infrastructure description \(ansible files\)
* docker containers \(docker benchmark\)

When integrating the tools in a CD/CI pipeline, each attack vector should be considered seperately. Thus we would create a job in the build pipeline for each specific attack vector. Cover at least the OWASP top ten attack vectors step by step, one job at a time.

## The tools

### nikto

nikto is a web server assessment tool. It’s designed to find various default and insecure files, configurations, and programs on any type of web server.

Get nikto 

`apt-get update && apt-get install nikto -y`

create the output file for the scan

`touch /usr/share/doc/nikto/nikto.dtd`

run nikto against your webapp

`nikto -output nikto_output.xml -h localhost`

### nikto: CD/CI integration

```text
dast-webserver-check:
  stage: integration
  image: ubuntu:latest
  before_script:
    - apt-get update && apt-get install nikto -y
    - touch /usr/share/doc/nikto/nikto.dtd
  script:
    - nikto -output nikto_output.xml -h http://localhost
  artifacts:
    paths: [nikto_output.xml]
    when: always
    expire_in: one week
  allow_failure: true
```

### nmap

Nmap is the most powerful and popular network scanning tool. 

Install and run

`apt-get update && apt-get install nmap -y`

`nmap hostname/ip -oX nmap_out.xml`

`cat nmap_out.xml`

### nmap: CD/CI integration

```text
dast-nmap-check:
  stage: integration
  image: ubuntu:latest
  before_script:
    - apt-get update && apt-get install nmap -y
  script:
    - nmap localhost -oX nmap_out.xml
  artifacts:
    paths: [nmap_out.xml]
    when: always
    expire_in: one week
  allow_failure: true

```

### SSLyze <a id="title"></a>

SLyze is a fast and powerful SSL/TLS scanning library. It allows you to analyze the SSL/TLS configuration of a server by connecting to it, in order to detect various issues \(bad certificate, weak cipher suites, Heartbleed, ROBOT, TLS 1.3 support, etc.\). SSLyze can either be used as command line tool or as a Python library. [https://github.com/nabla-c0d3/sslyze](https://github.com/nabla-c0d3/sslyze) Installation goes 

`pip3 install sslyze`

A possible scan with a json output on hostname and port 443 would be

`sslyze --regular --json_out sslyze-output.json <hostname:443>`

**–regular** flag used to tells tool to perform Regular HTTPS scan

### ssLyze: CD/CI integration

in gitlab. 

```text
dast-ssl-check:
  stage: integration
  image: python:3.6
  before_script:
   - pip3 install sslyze
  script:
    - sslyze --regular --json_out sslyze-output.json localhost:443
  artifacts:
    paths: [sslyze-output.json]
    when: always
    expire_in: one week
  allow_failure: true
```

### zap

ZAP is an open-source web application security scanner to perform security testing \(Dynamic Testing\) on web applications. OWASP ZAP is the flagship OWASP project used extensively by penetration testers. ZAP can also run in a daemon mode for hands-off scans for CI/CD pipeline. ZAP provides extensive API \(SDK\) and a REST API to help users create custom scripts.

Pull the docker image provided by OWASP

`docker pull owasp/zap2docker-stable`

To run the baseline use the python script zap-baseline.py described here [https://www.zaproxy.org/docs/docker/baseline-scan/](https://www.zaproxy.org/docs/docker/baseline-scan/)

`docker run --rm owasp/zap2docker-stable zap-baseline.py`

Run the baseline against your \(remote\) host

`docker run --rm owasp/zap2docker-stable zap-baseline.py -t https://localhost:8080`

Run a more advanced scan with output

`docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable zap-baseline.py -t https://localhost:8080 -J output.json`

Explanation here:

```text
docker run 
    --user $(id -u):$(id -g)        # run container with the current user
    -w /zap                         # working directory inside the container
    -v $(pwd):/zap/wrk:rw           # map current directory $(pwd) to /zap/wrk in the container with rw access
    --rm owasp/zap2docker-stable    # remove container after quitting
    zap-baseline.py                 # run baseline tests
    -t https://localhost:8080       # run against this host
    -J output.json                  # output the results to this jason file
    
```

### zap: CD/CI integration

in gitlab. Note we'll do the same commands as o our local machine. After the script has run we'll free up some space by doing docker rmi

```text
zap-scan:
  stage: integration
  script:
     - docker pull owasp/zap2docker-stable
     - docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable zap-baseline.py -t https://localhost:8080  -J zap-output.json
  after_script:
     - docker rmi owasp/zap2docker-stable
  artifacts:
    paths: [zap-output.json]
    when: always
    expire_in: one week
  allow_failure: true
```

