# Web Application Security Testing

## Manual Web Application Security Hacking Methodology

Web Application Security Testing consists always of the following steps:

1. foot printing - passively gaining information. It is one of the most important step. The more information you get, the better and the more preciesely you can attack your target. A good preparation is always important. See [Footprinting tools](untitled.md) for more information.
2. scanning - mapping the network. See [Scanning networks](untitled-1.md)
3. enumeration - finding vulnerabilities. Goes hand in hand scan to get info about services, server, version, os, ip adresses, ports. Tools are: armitage, creates a map of scanned machines, further enums possible. More [Enumeration](enumeration.md)
4. gaining access - use of a penetrating tool: searchsploit, msf, armitage check exploits 
5. maintain access - setting up backdoors is difficult, schedule a service which will open a backdoor or set up a listener or design a script. For example netcat session start up then cover tracks. 
6. covering tracks - altering logs and hiding activity, delete files, scheduled services, user accounts, logs
7. writing a report. See also [Vulnerability assessment](vulnerability-assessment.md)

A lot of the mentioned tools have integrated automated testing such as automated checks against vulnerability databases, predefined attack payloads and predefined attacks that one can use out-of-the-box. 

### Get the tools - Pentesting OS compairison

How to get to the tools? By using an especially for the purpose of penetration testing designed OS. The most popular pentesting OS on the market as of today are listed here. As a pentester, you should have seen an instance of each. Install all of them in virtual machines and play with it. If you plan to certify in pentesting, I recommend to use the OS of the school. EC-Council uses parrot, Offensive-Security kali for instance. BlackArch is the most difficult to manage.

<table>
  <thead>
    <tr>
      <th style="text-align:left">criteria</th>
      <th style="text-align:left">kali</th>
      <th style="text-align:left">parrot</th>
      <th style="text-align:left">blackarch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">RAM usage</td>
      <td style="text-align:left">450MB</td>
      <td style="text-align:left">550MB</td>
      <td style="text-align:left">170MB</td>
    </tr>
    <tr>
      <td style="text-align:left">Based on OS family</td>
      <td style="text-align:left">Debian</td>
      <td style="text-align:left">Debian</td>
      <td style="text-align:left">Arch Linux</td>
    </tr>
    <tr>
      <td style="text-align:left">Recommended user interface</td>
      <td style="text-align:left">Gnome&amp;xfce</td>
      <td style="text-align:left">KDE&amp;Mate</td>
      <td style="text-align:left">
        <p>XFCE or</p>
        <p>none (light version)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Anonymous mode</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">anonsurf</td>
      <td style="text-align:left">tor</td>
    </tr>
    <tr>
      <td style="text-align:left">Space on disc</td>
      <td style="text-align:left">1GB</td>
      <td style="text-align:left">320Mb</td>
      <td style="text-align:left">N/A</td>
    </tr>
    <tr>
      <td style="text-align:left">Tools preinstalled</td>
      <td style="text-align:left">400</td>
      <td style="text-align:left">600</td>
      <td style="text-align:left">2676</td>
    </tr>
    <tr>
      <td style="text-align:left">Updates</td>
      <td style="text-align:left">frequent,stable</td>
      <td style="text-align:left">frequent,stable</td>
      <td style="text-align:left">very quickly,instable</td>
    </tr>
    <tr>
      <td style="text-align:left">Configuration</td>
      <td style="text-align:left">easy</td>
      <td style="text-align:left">easy</td>
      <td style="text-align:left">hard</td>
    </tr>
    <tr>
      <td style="text-align:left">Used by school</td>
      <td style="text-align:left">offensive-security</td>
      <td style="text-align:left">EC-council</td>
      <td style="text-align:left">N/A</td>
    </tr>
  </tbody>
</table>

So, to be good at web application security testing, you should know in that exact order: common vulnerabilities and attacks, where to find them, choose a penetration testing OS, know the tools and apply them according to your methodology.

### More methodologies

OWASP has also defined a Web Testing Framework that you can use to conduct a guided pentest. Follow instructions on [https://github.com/owtf/owtf](https://github.com/owtf/owtf)

{% embed url="https://hackmethod.com/hacker-methodology/?v=11aedd0e4327" %}

## 

## Automated Web Application Security Testing

Different types of testing:

SCA, Software Component Analysis

SAST, Static Application Security Testing

DAST, Dynamic Application Security Testing

OAST, Out-of-Band Application Security Testing

System Hardening

System Compliance Testing





