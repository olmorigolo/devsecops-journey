# Vulnerable Web Apps

## Java

{% embed url="https://github.com/CSPF-Founder/JavaVulnerableLab" %}

I recommend installing it with docker \(you need docker-compose as well\). Inside the project directory run

```text
$ docker-compose up
```

### WebGoat Java Web App

`git clone https://github.com/hamhc/WebGoat-7.1.git webapp`

## Python

Get [https://github.com/stamparm/DSVW](https://github.com/stamparm/DSVW) or a dockerized app [https://github.com/anxolerd/dvpwa](https://github.com/anxolerd/dvpwa)  

## Php

Damn Vulnerable Web App. To get it on your machine I recommend to use the docker image, so you don't need to configure it [https://hub.docker.com/r/vulnerables/web-dvwa/](https://hub.docker.com/r/vulnerables/web-dvwa/)

```
$ docker run --rm -it -p 80:80 vulnerables/web-dvwa
```

## Javascript

Juice Shop is written in Node.js, Express and Angular. Get it on [https://github.com/bkimminich/juice-shop](https://github.com/bkimminich/juice-shop)

```text
docker pull bkimminich/juice-shop
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

{% hint style="success" %}
More vulnerable web apps can be found in a curated list here [https://owasp.org/www-project-vulnerable-web-applications-directory/](https://owasp.org/www-project-vulnerable-web-applications-directory/)
{% endhint %}

