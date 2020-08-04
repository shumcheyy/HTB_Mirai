# HTB_Writeups
My writeup on how I got the root and user flags for Hack the Box machine Mirai

### Nmap Scan

nmap -A -p- -T4 10.10.10.48 -v -oA Mirai/

The below Nmap scan shows that 6 ports are open
![mirai_nmap](https://user-images.githubusercontent.com/29353729/89279576-2b04d780-d665-11ea-8e83-8aa0ca071313.jpg)

We find that there are 2 ports which are hosting web server {80,32400}

Lets find out what both of them give

