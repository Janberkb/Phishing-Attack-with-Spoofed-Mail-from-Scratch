# Credentinal Harvesting Over Website and E-Mail Scam Attack Scenario

## Content

- Introduction
- Lab Installation Guide
- Simulation Workflow
- Conclusion

## Introduction

I decided to setup local virtual lab to try phishing techniques and create a scenario about it. So I started to research about email scam because phishing over website and credentinal harvesting has one goal; take the credentinal, send it back to me and save this informations to the file. I find some tool for sending mail using SMTP. Let's setup the local lab.

## Lab Installation Guide

### R**equirements**

- Kali Linux,
- Windows 10 device for recieving mail and hMailServer installed as SMTP Server,
- PHP installed on your kali,
- Gophish installed on your kali,

### Installation

1. Kali Linux Installation: 1. [https://www.kali.org/docs/installation/hard-disk-install/](https://www.kali.org/docs/installation/hard-disk-install/) 
    
                                  2. [https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)
    
2. Windows 10 Installation: [https://www.groovypost.com/howto/windows-10-install-virtualbox/](https://www.groovypost.com/howto/windows-10-install-virtualbox/)
3. Install "hMailServer": [https://medium.com/@coffmans/setup-your-own-simple-smtp-server-how-to-c9159cfc7934](https://medium.com/@coffmans/setup-your-own-simple-smtp-server-how-to-c9159cfc7934)

Actually in real scenario we just need an SMTP Server for sending mail but we are going to see how the mail comes. So we need to setup an entire mail system including imap.

When we setup the "hMailServer" we can open mailbox using Outlook or Mail app of Microsoft:

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled.png)

Create a domain and account like installation document shows you and when open Outlook just type your account that you created.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%201.png)

After that you need to choose IMAP from here.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%202.png)

Fill this server fields with your IP of your Windows machine because hMailServer is installed and working on this machine. It needs to go there, send and recieve mails from that server.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%203.png)

Type the password of the user and voilà :) You have a mailbox. Now you can send and recieve mail locally. 

Now we need to send spoofed mail so we need a tool called gophish, I gave the installation link above when you install gophish on your kali linux everything will be perfect and we are going to be ready to go.

<aside>
💡 Troubleshooting Tips: If your outlook doesn't connect to your mailbox you need to give permission to these ports from Windows Firewall Advanced Settings Inbound Rules (25, 110, 143)

</aside>

1. Gophish Install: [https://github.com/gophish/gophish/releases](https://github.com/gophish/gophish/releases)

Download the lastest gophish release and extract from archive.

Go to the folder that you have extracted on command line and set permission to the executable file.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%204.png)

Now you can run the executable file.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%205.png)

It opens a user interface on 127.0.0.1:3333 and phishing server at 0.0.0.0:80 and it gives us the one time admin password for admin interface.

Login with that one time password to admin account and set a new password.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%206.png)

Because of we will use our own phishing website we need to change the port of gophish's landing page from 80 to 8080

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%207.png)

Open the config.json file with text editor.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%208.png)

Change the listen_url value under phish_server from 0.0.0.0:80 to 0.0.0.0:8080

We are doing this because we need to share our phishing website from port 80 and there should be no service on port 80. 

### Simulation Workflow

This is the most important part, we are going to see the workflow of our scenario. Let's quick check;

We have a Windows 10 machine which is victim's computer but remember this is simulation and we need an SMTP Server so we are using this victim's machine as Mail Server and we installed "hMailServer" installed on this machine.

We have a Kali Linux machine for hacking of course :) We installed php on this because it will publish our phishing website for other users and catch the POST request. We have gophish installed on kali too and gophish is a tool to create mail phishing simulations as you can guess. 

Now I am ready to create my mail template as HTML.

We are going to attack over instagram and we need a mail like "New login to your account, if it is not you, you can secure your account from this link". So I am using the mail from my mailbox :)

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%209.png)

It is Turkish mail but it is saying "You have login from the device new, you can ignor this mail if it was you, if you don't do that you can secure your account from here." and something like that.

Open your developer tool and choose the mail content.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2010.png)

Right click to this element and copy as HTML.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2011.png)

Go to Gophish's e-mail template tab and create a new e-mail template.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2012.png)

Paste this HTML code to HTML on here, after this we just need to change texts values maybe pictures and it is on your skills. You need to put a link here for phishing website. We are simulating so I have changed my Windows's host file and add there "www.instegram-secure-your-account-6wf35.com.tr". You can add files to your mail if you want.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2013.png)

I am running this website on my local network so I wrote my kali's ip because I will stream my website on it.

Save all of these settings and turn back to gophish settings.

Go to Sending Profiles and create a new profile.

 

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2014.png)

Enter your mail address that you want to show to victim and you need to type your smtp server address and username password if there is but in our lab we don't have username password.

After all of this tiredness you just need to create user group and add user in it and create a campagin.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2015.png)

<aside>
💡 You need to create a landing page too but we are not going to use it you need to leave it blank. If you don't create empty landing page gophish is not working on campaign step.

</aside>

Send a test mail and see what is going on :)

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2016.png)

Now, we can send spoofed mail. It is time to prepare our phishing website.

I am going to do it on easy way and find a phishing website tool, we can use their templates of course. I will install nexphish for this but I will just take its template.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2017.png)

As you can see there are hidden folders, ".Modules" folder has a lot of website template and "login.php" file. When I inspect that files a little bit I notice that they are just sending user credentinals with POST protocol and save in "usernames.txt" file in existing folder.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2018.png)

Now we need to open our console here and publish this website with the code below.

```jsx
sudo php -S 0.0.0.0:80
```

Now we can access this website from Windows machines if we are in the same network. 

Remember we typed something in "hosts" file on Windows machine, that was kali's IP and the website link of our website.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2019.png)

Let's start the campaign and see if this phishing works.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2020.png)

This panel shows you all the things about campaign.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2021.png)

Hey! We have recieved a mail from security@mail.inst(e)gram.com :) 

When we click the link.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2022.png)

When we enter our credentinals (Not the real ones) it redirects us to real instagram page. We can check the usernames file in the website folder and see the credentinals.

![Untitled](Credentinal%20Harvesting%20Over%20Website%20and%20E-Mail%20Sca%208b60b348221b4f0eb8a23762e1fdf2bd/Untitled%2023.png)

## Conclusion

> If you have recieved a mail like this and you need to change your password, open the browser and change your password manually, not with sent link and the most important thing, use two factor authentication. Two factor authentication is so hard to brake, of course nothing is impossible but you need to becareful and take as many precautions as you can.
> 

<aside>
💡 I am waiting for your feedbacks about this blog post, please let me know If I have a mistake. Feel free about pull requests.

</aside>