---
layout: post
title: "Setup your system's mail to send emails via GMail"
data:   2015-08-16 15:25:00
author:  Abhishek Bhattacharjee
---

This has been a pain for me, for past few weeks, I had to dig through a lot of websites
to get my mail working, so I thought of putting it all together.
I bumped into this issue when I needed [caff](https://wiki.debian.org/caff) to sign keys
and send emails to people I signed the keys of.

Keysiging party at this years DebConf, i.e [DebConf '15](http://debconf15.debconf.org/)
drove me to setup my system's mail. Now I can send keysigning emails using caff through
my system's mail using gmail's smtp server.

To setup the same on your system((Debian 8, Jessie)), follow the following steps:

### Step 1
Run the following command as root

	dpkg-reconfigure exim4-config


After hitting enter, you will see a window appear on the terminal screen

* Select "mail sent by smarthost; recieved via SMTP or fetchmail" and hit enter
* Next write "gmail.com" for "System mail name"
* Next put "127.0.0.1" in filed "IP-address to listen on for SMTP connections"
* Leave the field "Other destinations for which mail is accepted" completely empty
* Leave the "Machines to relay mail for" empty
* Now write "smtp.gmail.com::587" for the field "IP address or host name of the outgoing smarthost"
* Hit "Yes" for "Hide local mail name in outgoing mail ?"
* Keep "Visible domain name for local users" as "localhost"
* Hit "No" for the field "Keep number of DNS-queries minimal"
* Select "mbox format in /var/mail/"
* Select "Yes" for "Split configuration into small files?"

### Step 2
Open the file **/etc/mailname** and remove "localhost" and write the following:

	gmail.com

Save the file

### Step 3
Open **/etc/exim4/passwd.client** and add the following line:

	*.google.com:YourUserName@gmail.com:YourGmailPassword

Save the file

### Step 4
Edit **/etc/email-addresses** and add following line:

	Your_User_Name: UserName@gmail.com

Save the file

### Step 5
You need to make your gmail account's **less secure app access** on.
To do that, login to your gmail account and then visit the following link:

[Less Secure App access](https://www.google.com/settings/security/lesssecureapps)

**Turn it on**

### Step 6
Execute the following commands being root

	$ update-exim4.conf
	$ invoke-rc.d exim4 restart
	$ exim4 -qff

### Step 7
Check your system's mail client uses gmail to send emails.
For that run the following command(not root):

	$ mail -s "Subject for the mail" receiver@gmail.com \
	-aFrom:Your\ Name\<your-email@gmail.com\>


* Hit enter
* Write your message
* Press Ctrl-D, **Cc** field will appear

Prompt should look like this:

	Your message

	<Ctrl-D> (you press it, not visible in terminal)

	Cc:

After hitting enter(i.e leaving Cc field emtpty), prompt will return.

This should send an email to the given email id(here, receiver@gmail.com).
Go to your gmail account to check sent mails, it should up there.
Also, check with the person you sent the test mail to.

Some of the steps could be superfluous, but they did work for me.
Also **Step 5** is very important for this to work.

**Step 5** could make you uncomfortable, but you can turn off the feature whenever you want.
It is better to switch 'less secure app access' off, when you are not using your system's mail
for mailing purpose.

Having your system's mail client able to send emails through GMail is very useful
where you've only gmail as your primary email and you want to sign keys **;-)**
