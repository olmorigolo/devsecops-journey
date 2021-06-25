---
description: Maturity Model Level 2
---

# Static Application Security Testing \(SAST\)

## What is it?

SAST tools analyse the data and control flow of an application. They require manual analysis as they'll find many false positives. SAST tools won't find runtime bugs. SAST should be run on

* source code
* configuration \(linting\)
* infrastrcuture as code \(linting\)
* docker containers

## The tools

A good summary of tools is available here: [https://github.com/analysis-tools-dev/static-analysis](https://github.com/analysis-tools-dev/static-analysis) and here  [https://analysis-tools.dev](https://analysis-tools.dev/)

### bandit: usage

The Bandit is a tool designed to find common security issues in Python code. To do this Bandit, processes each file, builds an AST, and runs appropriate plugins against the AST nodes. Once Bandit has finished scanning all the files it generates a report. The project is available at [https://github.com/PyCQA/bandit](https://github.com/PyCQA/bandit).

Again enter the root folder of our vulnerable app and do

`pip3 install bandit`

Execute the scan with

`bandit -r . -f json > bandit-output.json`

### bandit: CD/CI integration

```text
sast:
  stage: test
  image: python:rc-alpine3.13
  before_script:
    - apk add git
    - git checkout master
    - pip3 install bandit
  script:
    - bandit -r . -f json > bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
    expire_in: one week
  allow_failure: true
```

### breakman

You need ruby running on your host machine. Update your system and  install Ruby with the following command.

`apt update && apt install ruby-full -y`

Install brakeman

`gem install brakeman`

Run the scanner and output the results into  a json file

`brakeman -f json | tee result.json`

You can add a ignorefile to the scan by creating a file with the extension .ignore

`nano brakeman.ignore`

Example content:

```text
{
    "ignored_warnings": [
        {
          "fingerprint": "febb21e45b226bb6bcdc23031091394a3ed80c76357f66b1f348844a7626f4df",
          "note": "ignore XSS"
        }
    ]
}
```

run the scan with the ignorefile:

 `brakeman -f json -i brakeman.ignore | tee result.json`

### Secret scanners

Secret scanners will analyse the code in search for credentials. Types of secret scanners are

* entropy based: look for random data and lacks predictability.

  -- catches unknown issues

  -- no custom rules

  -- needs to be random

  -- not always possible

* regex based: look for known secrets patterns in code.

  -- lots of false positives

  -- one can do custom regex

  -- catches known issues

### trufflehog: usage

truflleHog is a tool that searches through git repositories for secrets, digging deep into commit history and branches. This tool is useful in finding the secrets accidentally committed to the repo. You can find more details about the project at [https://github.com/dxa4481/truffleHog](https://github.com/dxa4481/truffleHog).

Again you execute the command within the root folder of your python web app.

`pip3 install trufflehog`

`trufflehog --json . > secret.json`

### trufflehog: CD/CI integration

```text
git-secrets:
  stage: build
  image: python:rc-alpine3.13
  before_script:
    - apk add git
    - git checkout master
    - pip3 install trufflehog
  script:
    - trufflehog --json . | tee secret_results.json
  artifacts:
    paths: [secret_results.json]
    when: always
    expire_in: one week
  allow_failure: true   #We don't want to fail jobs in maturity level 1 and 2, lots of false positives
```

### GoSec

GoSec allows tatic analysis scans on Golang code. The project is located at [https://github.com/securego/gosec](https://github.com/securego/gosec)

Test the tool on a golang repo

```text
git clone https://gitlab.practical-devsecops.training/pdso/golang.git webapp
```

Install Go on your machine

If you don't have go on your machine, make it ready with by installing go by following the instructions at [https://golang.org/doc/install](https://golang.org/doc/install)

You should also set the following environment variables

```text
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

Install Gosec

```text
go get github.com/securego/gosec/v2/cmd/gosec
```

Run your first scan with

```text
gosec ./...
```

You can ignore findings with

```text
gosec -exclude=G104 ./...
```



### njsscan

A tool that runs static analysis scan on a NodeJS code. Download and install it from the project at [https://github.com/ajinabraham/njsscan](https://github.com/ajinabraham/njsscan) or install it directly using the python installer

```text
pip install njsscan
```

Run the scanner

```text
njsscan --json -o output.json /myNodeJsProject
```

### pylint

A tool that runs code quality scans on Python code.

Install it with pip

```text
pip3 install pylint
```

Run the scanner on .py files in your project folder called myPythonApp

```text
pylint myPythonApp/*.py
```

You can output results as json to a file like this

```text
pylint myPythonApp/*.py -f json | tee output.json
```

Reduce False Positive

The `--generate-rcfile` option will generate a commented configuration file on standard output according to the current configuration and exit. Store the content into .pylintrc which is also your ignore file.

### Semgrep

### SonarQube

