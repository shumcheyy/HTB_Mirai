# HTB_Writeups
My writeup on how I got the root and user flags for Hack the Box machine Mirai

![mirai](https://user-images.githubusercontent.com/29353729/89324290-b05bac80-d6a4-11ea-9a23-21cac4824ff8.png)

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

![mirai_admin](https://user-images.githubusercontent.com/29353729/89321847-10e8ea80-d6a1-11ea-9da2-533f7838b49c.png)

we find a web interface that shows us statistics and logs and stuff

On the left panel we find login tab

![admin_login](https://user-images.githubusercontent.com/29353729/89322057-54435900-d6a1-11ea-8ba8-795376f51450.png)

We try to login , {we are pi}.
We can try bruteforcing or search for default creds for Pi-Hole

https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md


Using this info we try to login in the web interface but can't get in

So we try as ssh with {pi:raspberry} as login creds

ssh {pi@10.10.10.48}
{raspberry}

![mirai_ssh](https://user-images.githubusercontent.com/29353729/89322173-7dfc8000-d6a1-11ea-89f7-06c3649e54ea.png)

We are in

pi is one of the users of the system

We search through the system and find the user flag from

cat /home/pi/Desktop/user.txt
###### ff837707441b257a20e32199d7c8838d

Great! We have the user flag now.Time to get the root flag

Now to access root

we first try sudo -i

and we get the root shell.Nice we are root now

Finding the flag should be easy now

we go with 

cat /root/root.txt

What? We dont have the flag? It says

_"I lost my orignal root.txt! I think I may have a backup on my USB stick"

Alright we know from where we can get in the mounted devices

It's 
cd /media/usbstick

we find the file damnit.txt

It says it deleted the file from here too.

Bummer!!

Now what are we supposed to do

The file says if we can somehow recover it, it should be good

So how do we recover files in Linux?

We do have some 3rd party tools available , but we don't need them in this case

Remember , everything in Linux is a file , just all of them are treated differently.

Honestly , I didn't know what to do now even with the above knowledge , didn't know where to go from here

So I tried using different inbuilt tools/commands of Linux like

fdisk , df , du , lsblk

wanted to see how the device was mounted.

My other option was to read Logs but it might have taken too long so I kept it for later

using the command 

###### fdisk -l /dev/sdb

showed the details of the device

Then I tried to 

cat /dev/sdb

But it wall all gibberish and there was so much data

One habit of mine is to apply the strings command on most of the files that show gibberish (got this habit by playing ctf}

Still didn't get anything of info

I wanted to reduce the data being showed if possible

One of the options of the strings commands is 
###### strings -a{f}

![mirai_root](https://user-images.githubusercontent.com/29353729/89323908-0da32e00-d6a4-11ea-9e7e-efe2b98e6f5d.png)

This gave us a big number

At first I didn't know this was the flag

Just entered the number and it was accepted

root flag
###### 3d3e483143ff12ec505d026fa13e020b
