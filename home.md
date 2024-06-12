# Report Cybersecurity
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
