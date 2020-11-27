---
layout: post
title: Win10 OpenSSH Server and Config Remote Jupyter 
---

## 1. Enable ssh and ssh server on Windows 10
    
On Windows 10 system, the SSH client is installed by default. But OpenSSH Server is uninstalled which is installed On Linux(like Ubuntu) by default. So that's why we can not use `ssh user-name-on-remote-sys@ip-address-of-remote-sys` to connect to the Windows machine directly.

Here are some pictures about OpenSSH Server installation on Windows 10, we can extend the application just by using optional symbol. 

<center class="half">
    <img src="../images/openSSH/OpenSSH.png" width="250" height="220"/><img src="../images/openSSH/openssh_installed.png" width="110" height="220"/>
</center>

That is very important step, if you close the service of Windows Update, you will not install the OpenSSH Server successfully.(For me, I just close the Update because I don't my machine reboot automatically during the night). So make sure that it is enabled.

<center class="half">
    <img src="../images/openSSH/winupdate.png" width="250" height="220"/>
</center>

You need administrator privileges to enable services so open Powershell as Administrator, (right click on the win icon in the bottom)

<center class="half">
    <img src="../images/openSSH/Powershell.jpg" width="200" height="300"/>
</center>

And then, follow these commands:

![](../images/openSSH/command.png)

The last thing to check is the firewall setting for sshd. It by default uses the port number 22. Enabling the service automatically created the following firewall rules,

![](../images/openSSH/firewall.png)

Note: If you enable sshd you are creating an "open port" for port 22. (Otherwise you wouldn't be able to connect to it.) If your system is exposed to the outside world then that might bother you. You can do things to tighten up security like disallowing passwords and requiring only "public-key" access. I'm not going to cover any of that here. If you are on a private LAN you don't have too much to worry about, but always be security conscious and use good passwords!


## 2. Start Jupyter notebook with --no-browser and --port

There are two ways to start jupyter notebook.
- config the file

![](../images/openSSH/cmd.png)

 At first generate the config file of jupyter using `jupyter notebook --generate-config`.

 Then you will find the file in the folder,

 <center class="half">
    <img src="../images/openSSH/configfile.png"/>
</center>

you can open it as txt, and find these position, change them as follow and save:

<center class="half">
    <img src="../images/openSSH/position.png" width="300" height="250"/>
</center>

Finally, these operation can be replaced by the second way when you just use `jupyter notebook` as command.

- `jupyter notebook --no-browser --port 8899`

(The way just type more words!)

## 3. Connect to windows 10 machine

Here, I start the jupyter service on the windows 10 machine which is located at school using jupyter notebook with config file.

Because of the same LAN, I just use the follow command to connnect on my Linux Laptop.


![](../images/openSSH/sshcommad.png)

After I input the password, I start the remote jupyter service on my laplop, just using the server's rescourse.

![](../images/openSSH/jupyter.png)

# BAM!!!




