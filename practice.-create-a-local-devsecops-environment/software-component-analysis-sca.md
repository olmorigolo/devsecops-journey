---
description: Maturity Model Level 1
---

# Software Component Analysis \(SCA\)

## What is it?

This analysis should run locally on each dev instance. How SCA tools work? SCA tools look for components/library/dependency files in the repository. Each dependency is crossed checked using its checksums with a database of vulnerable components. Every programming language has its own package/dependency manager and build tool.

Some build tools and dependency managers can be found in this table.

| Language | Dependency Manager & Build tool | File |
| :--- | :--- | :--- |
| Python | pip | requirements.txt |
| node.js, | npm | package.json |
| java | rubygems | gemfile |
| java | maven | pom.xml |
| java | gradle | build.gradle |
| java | ant | .xml |
| haskell | cabal | `.cabal` |

## The tools

Fall under SAST. List available here [https://analysis-tools.dev/](https://analysis-tools.dev/) and here: [https://github.com/analysis-tools-dev/static-analysis](https://github.com/analysis-tools-dev/static-analysis) 

## CD/CI pipeline

We do not want to fail the builds in **DevSecOps Maturity Levels 1 and 2**. If a security tool fails a job, it won’t allow other DevOps jobs like release/deploy to run hence causing great distress to DevOps Team. Moreover, the security tools suffer from false positives. Failing a build on inaccurate data is a sure recipe for disaster. In gitlab pipelines one can use the **allow\_failure** tag to not fail the build even though the tool found security issues.

Rule of thumb: only fail the build when **CVSS** score is lower than **7** and save the output preferably as **JSON** if available.

### retire.js: usage

Retire.js is a tool to analyse components in a nodejs project, by default based on the file package.json. The juiceshop [https://github.com/bkimminich/juice-shop](https://github.com/bkimminich/juice-shop) application is based on nodejs. We'll use [https://github.com/RetireJS/retire.js](https://github.com/RetireJS/retire.js) for SCA.

Let's play this locally: get the vulnerable web app on your machine/inside your container and enter its root folder

`$ git clone`[`https://github.com/bkimminich/juice-shop`](https://github.com/bkimminich/juice-shop.git)\`\`

`$ cd juice-shop`

Install node js $

`$ curl -sL https://deb.nodesource.com/setup_10.x | bash - $ apt install nodejs -y`

Install the  SCA tool 

`$ npm install -g retire`

Run the scanner. The scanner will then scan the javascript libraries/depencies found in the package.json file:

`$ cat package.json`

Before running the scan, we need to install the npm packages available in the package.json file using the npm install command.

`$ npm install`

Run retire and store the result in a machine readable format. 

`$ retire --outputformat json --outputpath output.json`

If that worked, we can now integrate the scanner into our build pipeline. We need to describe the steps we did locally in the build script. Note that we did not have to get an environment to run our tool locally, since it was our local computer. If we integrate the tool in a build pipeline we should get a docker image where we can run this tool. If you want, you can test this on your local machine first and do all of steps within a docker container. In the build script you need add a step to run the specific container first.

### retire.js: CD/CI integration

Integrate retirejs in the test stage of your build pipeline and allow failure. Create a .retireignore.json file in the root directory of your app if needed. If you don't have access to create it there you can pass a specific file using the option --ignorefile .myignorefile.json

Example of an integration in a gitlab CD/CI

```text
sca-analysis:
  stage: test
    image: node:alpine3.10
    before_script:
      - npm install
      - npm install -g retire # Install retirejs npm package.
    script:
      - retire --outputformat json --outputpath retirejs-report.json --severity high --exitwith 0
    artifacts:
      paths: [retirejs-report.json]
      when: always # What is this for?
      expire_in: one week
    allow_failure: true
```

We allow the failure in SCA analysis.

### npm: usage

NPM has security build in since npm@6, this command allows you to analyze dependencies in your existing code to identify insecure dependencies, have a good security report, and also easy to implement it into CI/CD pipeline without install any packages except NPM itself.

install npm if not done

```text
sudo apt install npm
```

or update

```text
npm install npm@latest -g
```

install node js

`apt install nodejs -y`

`npm audit --json > results.json`

### npm: CD/CI integration

You should not run npm install before running the script because it will audit the package.js. If you want to break the build on failure you don't want to get the packages installed.

```text
npm-audit:
  stage: test
  image: node:alpine3.10
  script:
    - npm audit --json > npm_audit_results.json
  artifacts:
    paths: [npm_audit_results.json]
    when: always
    expire_in: one week
  allow_failure: true
```

### Safety: usage

Safety is a SCA tool for python projects. Download a vulnerable python based webapp and enter the root older of project

`git clone <your vulnerable web app> | cd <the root folder>`

Install safety and tun checks

`pip3 install safety`

tee command to store the output in a file and print out simultaneously 

`safety check -r requirements.txt --json | tee safety_output.json` 

or only store in a file

`safety check -r requirements.txt --json > safety_output.json`

### Safety: CD/CI integration

I did the practical devsecops certification and used their docker image to solve this.

```text
oast:
  stage: test
  script:
    # We are going to pull the hysnsec/safety image to run the safety scanner
    - docker pull hysnsec/safety
    # third party components are stored in requirements.txt for python, so we will scan this particular file with safety.
    - docker run -v $(pwd):/src --rm hysnsec/safety check -r requirements.txt --json > safety-output.json
  artifacts:
    paths: [safety-output.json]
    when: always
  allow_failure: true
```

{% hint style="danger" %}
If you choose to use open source scanners be sure they are safe. 
{% endhint %}

How can I be sure that my open source scanner does not contain malware? Either you rely on a community or you check-out the source code yourself and analyze the code. That's what github did [https://securitylab.github.com/research/octopus-scanner-malware-open-source-supply-chain/](https://securitylab.github.com/research/octopus-scanner-malware-open-source-supply-chain/)

### OWASP Dependency Check: usage

OWASP Dependency-Check is an open-source tool that checks dependencies for vulnerabilities of java projects. The tool is available on the owasp website here: [https://owasp.org/www-project-dependency-check/](https://owasp.org/www-project-dependency-check/)

Checkout a vulnerability App written in Java. [https://app.gitbook.com/@mw1po/s/devsecops/~/drafts/-M\_p7DakW5A1RcqLX09a/practice.-create-a-local-devsecops-environment/vulnerable-web-apps](https://app.gitbook.com/@mw1po/s/devsecops/~/drafts/-M_p7DakW5A1RcqLX09a/practice.-create-a-local-devsecops-environment/vulnerable-web-apps)

To run the owasp tool you need the java runtime JRE on your executing machine. Install it if not existent.

[`wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.1.6/dependency-check-6.1.6-release.zip`](https://github.com/jeremylong/DependencyCheck/releases/download/v6.1.6/dependency-check-6.1.6-release.zip)Extract it and add the executable to the path so you can use it on the command line

`export PATH=/opt/dependency-check/bin:$PATH`

Run it 

```text
dependency-check.sh --scan /webapp --format "CSV" --project "Webgoat" --failOnCVSS 8 --out /opt
```

### OWASP Dependency Check: CD/CI integration

create a file inside the app repo and copy paste the following code to it

```text
#!/bin/sh

DATA_DIRECTORY="$PWD/data"
REPORT_DIRECTORY="$PWD/reports"

if [ ! -d "$DATA_DIRECTORY" ]; then
  echo "Initially creating persistent directories"
  mkdir -p "$DATA_DIRECTORY"
  chmod -R 777 "$DATA_DIRECTORY"

  mkdir -p "$REPORT_DIRECTORY"
  chmod -R 777 "$REPORT_DIRECTORY"
fi

cd webgoat-container

# Make sure we are using the latest version
docker pull owasp/dependency-check

# mvn install -Dmaven.test.skip=true
docker run --rm \
  --volume $(pwd):/src \
  --volume "$DATA_DIRECTORY":/usr/share/dependency-check/data \
  --volume "$REPORT_DIRECTORY":/report \
  owasp/dependency-check \
  --scan /src \
  --format "CSV" \
  --project "Webgoat" \
  --failOnCVSS 8 \
  --out /report

```

You'll execute the file within the test step in the pipeline:

```text
odc-backend:
  stage: test
  image: gitlab/dind:latest
  script:
    - chmod +x ./run-depcheck.sh
    - ./run-depcheck.sh
  artifacts:
    paths:
      - reports/dependency-check-report.csv
    when: always
    expire_in: one week
```

### audit.js:usage

auditjs audits npm packages and identifies outdated packages using the api from Sonatype.

Installation

`curl -sL https://deb.nodesource.com/setup_10.x | bash -`

`apt install nodejs -y && npm install -g auditjs`

Running the help tells us about different scans:

```text
Commands:
  auditjs iq [options]    Audit this application using Nexus IQ Server
  auditjs config          Set config for OSS Index or Nexus IQ Server
  auditjs ossi [options]  Audit this application using Sonatype OSS Index
```

 But before we can run it, we need to install the dependencies of the project with 

`npm install`

`auditjs ossi -q -j | tee auditjs-output.json`

### audit.js: CD/CI integration

```text
  sca-auditjs:
    stage: test
    image: node:lts-alpine3.13
    script:
      - apk add git
      - git checkout master
      - npm install -g auditjs
      - npm install
      - auditjs ossi -q -j | tee auditjs-output.json
    artifacts:
      paths: [auditjs-output.json]
      when: always
      expire_in: one week
    allow_failure: true
```

### bundle-audit: usage

ls into the project folder of your ruby project and check the ruby version. You can use this app for testing `git clone https://gitlab.practical-devsecops.training/pdso/rails.git webapp`

cat Gemfile \| grep "ruby"

Now install it according to your system. Don't forget to add ruby on your path with 

```text
/webapp# export PATH="/root/.rbenv/versions/2.6.5/bin:$PATH"
/webapp# ruby --version
ruby 2.6.5p114 (2019-10-01 revision 67812) [x86_64-linux]
```

Then install the gem for bundle-audit

```text
gem install --user-install bundler-audit
```

Add bundle-audit to the path

```text
export PATH="~/.gem/ruby/2.6.0/bin/:$PATH"
```

Use it with 

```text
bundle-audit check --format json --output bundle-audit.json
```

Ignore vulnerabilities with criticity high by adding `.bundler-audit.yaml` to the repo

```text
---
ignore:
  - Criticality: High
  - CVE: xxxx-xxx
```



### bundler-audit CD/CI integration

find the ruby version of your app and get a corresponding docker image on docker hub

```text
sca-ruby-with-bundler:
  stage: test
  image: ruby:2.6.5-alpine3.11
  script:
    - apk add git
    - echo 'Checking out master branch ...'
    - git checkout master
    - echo 'Ruby version is'
    - cat Gemfile | grep "ruby"    # see ruby version
    - echo 'Installing bundler-audit sca tool...'
    - export PATH="~/.gem/ruby/2.6.0/bin:$PATH" # add ruby to the path
    - gem install --user-install bundler-audit
    - bundle-audit check --format json --output bundle-output.json # run check 
  artifacts:
    paths: [bundle-output.json]
    when: always
    expire_in: one week
  allow_failure: true
```







