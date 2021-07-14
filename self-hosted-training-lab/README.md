# Self-hosted Training Lab

I created my home penetration testing lab with existing virtual machines from penterster labs [https://pentesterlab.com/exercises](https://pentesterlab.com/exercises). You can download the ISO images direcly from their web page.

Once you run one of these machine, run `sudo ifconfig eth0 192.168.x.x` in a terminal to set a new ip address. Also think about setting the network of the VM with the vulnerable web app to host-only.

Another option is to install a vulnerable web app on your host machine if you know what you are doing. You don't want to break your host.  You can also install the web apps on a clean virtual machine:

{% page-ref page="vulnerable-web-apps.md" %}



