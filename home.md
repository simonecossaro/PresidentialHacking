# Report Cybersecurity : Presidential Hacking
Simone Cossaro IN2000201

## Introduction

This report presents the steps taken to solve the Vulnhub CTF (Capture the Flag) challange called Presidential-1. The attack was carried out following the walkthrough available [here](https://www.hackingarticles.in/presidential-1-vulnhub-walkthrough/).
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

## Enumeration

## Exploiting



## Privilege Escalation

The next step is to list the capabilities associated with the available executable files with the getcap command. this way we see that we have "+EP" in the tarS binary. This binary allows us to compress any file on the system without being root. Therefore, once compressed, we will only have to decompress it to be able to read the contents of the file.
There is an id_rsa file in the /root/.ssh/ directory. This file usually contains an RSA private key used for SSH authentication. This private key is associated with a public/private key pair and is used to authenticate the user on remote servers via SSH.
By having '+ep' we can compress the “id_rsa“ file, decompress it and gain visibility to the content.

The tarS -xvf id_rsa.tar /root/.ssh/id_rsa command creates a tar archive called id_rsa.tar containing the id_rsa file present in the /root/.ssh/ directory.

The tar -xvf id_rsa.tar command, however, extracts the files from the id_rsa.tar archive.

After executing these two commands, the id_rsa file can be read.

Now having the RSA private key, we can remotely connect to the shell on the presidential machine via the ssh protocol.
