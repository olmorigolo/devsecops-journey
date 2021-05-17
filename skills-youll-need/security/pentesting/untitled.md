# Scanning networks

## Tools

### nmap

An attacker always use a VPN, via the VPN a comromised machine, obfuscate the origin of your requests with -D.

Common nmap Commands

### smbmap

### nbtscan

### hping3

Common Hping Commands

* ICMP Ping: hping3 -1 10.0.0.25
* ACK scan on port 80: hping3 -A 10.0.0.25 -p 80 
* UDP scan on port 80: hping3 -2 10.0.0.25 -p 80 
* Collecting Initial Sequence Number: hping3 192.168.1.103 -Q -p 139 -s 
* Firewalls and Time Stamps: hping3 -S 72.14.207.99 -p 80 --tcp-timestamp 
* SYN scan on port 50-60: hping3 -8 50-60 -S 10.0.0.25 -V 
* FIN, PUSH and URG scan on port 80: hping3 -F -P -U 10.0.0.25 -p 80 
* Scan entire subnet for live host: hping3 -1 10.0.1.x --rand-dest -I eth0 
* Intercept all traffic containing HTTP signature: hping3 -9 HTTP -I eth0 
* SYN flooding a victim: hping3 -S 192.168.1.1 -a 192.168.1.254 -p 22 --flood

## ICMP echo reply

ICMP Ping for IP 10.0.0.23 with hping3 

```text
$ hping3 -1 10.0.0.23
```

Type 0 : echo reply; Type 8: Echo; Type 11: Time exceeded for a datagram; **Type 0: Destination unreachable:**

* 0 network unreachable
* 1 host unreachable
* 2 protocol unreachable
* 3 port unreachable
* 9 network comm admin prohibited
* 10 host comm admin prohibited
* 13 comm admin prohibited

\*\*\*\*



