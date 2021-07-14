# Web Application Security Testing

## Manual Web Application Security Hacking Methodology

Web Application Secrity Testing consists always of the following steps:

1. foot printing - passively gaining information. It is one of the most important step. The more information you get, the better and the more preciesely you can attack your target. A good preparation is always important.
2. scanning - mapping the network
3. enumeration - finding vulnerabilities. Goes hand in hand scan to get info about services, server, version, os, ip adresses, ports. Tools are: armitage, creates a map of scanned machines, further enums possible 
4. gaining access - use of a penetrating tool: searchsploit, msf, armitage check exploits 
5. maintain access - setting up backdoors is difficult, schedule a service which will open a backdoor or set up a listener or design a script. For example netcat session start up then cover tracks. 
6. covering tracks - altering logs and hiding activity, delete files, scheduled services, user accounts, logs 

A lot of the mentioned tools have integrated automated testing such as automated checks against vulnerability databases, predefined attack payloads and predefined attacks that one can use out-of-the-box.

## Automated Web Application Security Testing

Different types of testing:

SCA, Software Component Analysis

SAST, Static Application Security Testing

DAST, Dynamic Application Security Testing

OAST, Out-of-Band Application Security Testing

System Hardening

System Compliance Testing





