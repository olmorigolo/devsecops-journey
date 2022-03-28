# Web Security testing Methodology

## The WST methodology

1. foot printing - passively gaining information. It is one of the most important step. The more information you get, the better and the more preciesely you can attack your target. A good preparation is always important. See [Footprinting tools](untitled.md) for more information.
2. scanning - mapping the network. See [Scanning networks](untitled-1.md)
3. enumeration - finding vulnerabilities. Goes hand in hand scan to get info about services, server, version, os, ip adresses, ports. Tools are: armitage, creates a map of scanned machines, further enums possible. More [Enumeration](enumeration.md)
4. gaining access - use of a penetrating tool: searchsploit, msf, armitage check exploits&#x20;
5. maintain access - setting up backdoors is difficult, schedule a service which will open a backdoor or set up a listener or design a script. For example netcat session start up then cover tracks.&#x20;
6. covering tracks - altering logs and hiding activity, delete files, scheduled services, user accounts, logs
7. writing a report. See also [Vulnerability assessment](vulnerability-assessment.md)
