# HTB_Writeups
My writeup on how I got the root and user flags for Hack the Box machine Mirai

### Nmap Scan

nmap -A -p- -T4 10.10.10.48 -v -oA Mirai/

The below Nmap scan shows that 6 ports are open
![mirai_nmap](https://user-images.githubusercontent.com/29353729/89279576-2b04d780-d665-11ea-8e83-8aa0ca071313.jpg)

We find that there are 2 ports which are hosting web server {80,32400}

Lets find out what both of them give

http://10.10.10.48:80

![mirai_port_80](https://user-images.githubusercontent.com/29353729/89280975-00b41980-d667-11ea-8771-41a249ba0b7d.png)


It's just a blank page.I did a Nikto scan on it. Nothing is going on here apart from no-authentication header.Doesnt reveal much 

http://10.10.10.48:32400{/web/index.html}

![mirai_port_32400](https://user-images.githubusercontent.com/29353729/89281008-0d387200-d667-11ea-9c21-ce9d2ad748b3.png)

This webserver shows login page for Plex. Plex is a client–server media player system plus an ancillary software suite. The Plex Media Server desktop application runs on Windows, macOS, and Linux.

Again not much was found even with enumeration

Next thing to go for is spidering or bruteforcingly finding pages

I used dirbuster.

Dirbuster revealed a page /admin

http://10.10.10.48/admin/

{link}

we find a web interface that shows us statistics and logs and stuff

On the left panel we find login tab

Going there we find that we can login , don't know the username but we can enter a password

We can try bruteforcing or search for default creds for Pi-Hole

https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md


Using this info we try to login in the web interface but can't get in

So we try as ssh with {pi:raspberry} as login creds

ssh {pi@10.10.10.48}
{raspberry}

{link}

We are in

pi is one of the users of the system

We search through the system and find the user flag from

cat /home/pi/Desktop/user.txt
###### ff837707441b257a20e32199d7c8838d

sudo -i

since there is no password for root access

