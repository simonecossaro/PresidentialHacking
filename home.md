# Report Cybersecurity : Presidential Hacking
Simone Cossaro IN2000201

## Introduction

This report presents the steps taken to solve the Vulnhub CTF (Capture the Flag) challange called Presidential-1. 

The attack was carried out following the walkthrough available [here](https://www.hackingarticles.in/presidential-1-vulnhub-walkthrough/).

In the presented scenario the US state is developing the registration website for the presidential elections and wants to test the security of the server before the website and registration system are launched. One of the political parties is concerned that the other political party is going to perform electoral fraud by hacking into the registration system and falsifying the votes.

The goal of the challenge is to gain root access to the server.

## Tools

VirtualBox is the tool needed for this challenge to run the following two virtual machines:
* the Presidential VM available on Vulnhub [here](https://www.vulnhub.com/entry/presidential-1,500/)
* an attacking Kali Linux VM
  
The two virtual machines are connected to the same network with NAT.

## Recognition

The first step of the attack is reconnaissance. The shell command `netdiscover` automates the process of discovering live hosts on a network. Executing it will first allow us to find out if the presidential machine is connected to the network and if so what its IP address is. 

Once the IP address of the machine has been determined, we proceed by trying to identify the services and open ports and the software versions on it. The `nmap` command allows us to do this.

![Steps of recognition](images/reconnaissance.png)  

The scan shows that the machine is running a web service on port 80 with http protocol and a remote shell on port 2082 with ssh protocol.

## Enumeration

By visiting the main page of the web service on port 80, you will find an email address at the bottom (contact@votenow.local). This way we get a domain name: votenow.local.
It's possible to associate the domain name with the IP address by adding a line to the /etc/hosts file.

In this enumeration phase, the use of Gobuster, a tool for scanning directories and files on web servers, is essential. It is used to locate hidden or unadvertised resources on a web server, such as directories or files that may not be listed directly on a web page but may still be accessible.
The gobuster command specifies the file extensions to search for and the dictionary to use for brute force. Brute force in this context refers to systematically sending HTTP requests for each entry in the provided wordlist. Gobuster tries every word in the wordlist such as potential directory, file, subdomain, etc., until it finds valid resources or runs out of options.

![Steps of enumeration](images/enumeration.png)  

A config.php.bak file was detected. Usually these files contain a backup copy of the original config.php file. It in turn contains configuration information that may be important.
In fact, the file contains the user's credentials which could prove useful later.

## Exploiting



## Privilege Escalation

The next step is to list the capabilities associated with the available executable files with the getcap command. this way we see that we have "+EP" in the tarS binary. This binary allows us to compress any file on the system without being root. Therefore, once compressed, we will only have to decompress it to be able to read the contents of the file.
There is an id_rsa file in the /root/.ssh/ directory. This file usually contains an RSA private key used for SSH authentication. This private key is associated with a public/private key pair and is used to authenticate the user on remote servers via SSH.
By having '+ep' we can compress the “id_rsa“ file, decompress it and gain visibility to the content.

The tarS -xvf id_rsa.tar /root/.ssh/id_rsa command creates a tar archive called id_rsa.tar containing the id_rsa file present in the /root/.ssh/ directory.

The tar -xvf id_rsa.tar command, however, extracts the files from the id_rsa.tar archive.

After executing these two commands, the id_rsa file can be read.

Now having the RSA private key, we can remotely connect to the shell on the presidential machine via the ssh protocol.
