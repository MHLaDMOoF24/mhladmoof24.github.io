---
layout: post
title: "PostgreSQL setup"
tags: [production, database]
---

<span class="note">This post mostly concerns networking, but considering contact to your database should be important to you, this seems fair to still work on...</span>

## A new beginning
### Previously, on this blog...
We've chosen to go with a relational database for our project. One not connected to Microsoft, and therefore not T-SQL.  
PostgreSQL seems to be the best fit for our project, and is what we'll be using.  
Due to previous knowledge with SQL as a general language, I roughly know what to expect moving into it.  

A Redis database is still in consideration for timeblock caching, but not something I'm focusing on right now.  


### An alternative to the great Microsoft empire
One asset I procured last semester, for the previous project, was a VPS from the webhost LeaseWeb.  
Located in the Netherlands, it allowed us an escape from as much US influence as possible. The previous project didn't get to use it, but this time it's being set up from the very beginning.  
Having chosen to go with a Linux base install, we have seperated ourselves from both Windows and Azure at the same time.  


### Previous experience
Linux. I know basically nothing. Anders took a stab at it last semester, but we never got the full setup going for the project itself to use.  
It's also worth noting that we're using Ubuntu, as it's the most common distro, and therefore has the most documentation available.  

---

## The great frontier
### Setting up PostgreSQL... on Linux!
I knew I wanted to set up PostgreSQL on the VPS' Linux, but lacking previous experience with both systems, I had to look for sources. Luckily, Ubuntu's documentation has an article specifically for installing and configuring PostgreSQL.  
Article in question: [https://documentation.ubuntu.com/server/how-to/databases/install-postgresql/](https://documentation.ubuntu.com/server/how-to/databases/install-postgresql/)  

Between the article and asking all my dumb questions to ChatGPT, I managed to get PostgreSQL installed and running with little to no issue.  
Main concern turned out to be the networking side of things.  


### Networking woes
By default, PostgreSQL only listens to connections from localhost. This is a security measure, but also a hindrance when trying to connect from my local machine.  
Easy enough, change /postgresql.conf to listen_addresses = '*' (all IPv4 and IPv6). Put a pin in this enormous security risk for now.  
Then change /pg_hba.conf to allow connections from your own IP.  
And this is all covered in the Ubuntu article!  

... But also remember to open port 5432 (PostgreSQL's base port) in the Linux firewall, UFW.  
Still didn't work, so I checked the status of the UFW. And it was inactive.  

What's kept my VPS safe from the rest of the internet is LeaseWeb's own firewall. So I had to log into their control panel, and allow my personal IP.  
This gave me the hole I needed through.  

Returning to that pin from earlier, the Linux UFW and LeaseWeb firewall are the things keeping the database safe. Technically speaking, it should be fine as-is. But I would imagine that isn't the case everywhere, and if the VPS was used for more things than just hosting this database, allowing different IPs and ports would've made more sense. For now, we're good, but if we begin hosting an API there, this might have to change again.  


### Questions answered
PostgreSQL initially sets up with a superuser/root called postgres. This is _not_ the user to be using in day-to-day, or even general management operations on a particular database level. Instead, a different user is set up for the purpose of the particular database. The new user is the one I will be accessing from outside the VPS control panel, while the user "postgres" will be accessed solely from inside the VPS environment.  
Another initialization PostgreSQL does is setting up a default database called template1. I'm unsure of its actual purpose, but there's supposedly also a template0 (prestine version) below it. Create new databases around these, don't use them. Potentially template1 could be used as a template that you create others from? That would make sense, but I have not looked into it, and not doing so will likely not hurt me, as I'm setting up a single additional database for now. Might be something to look into later, though!  


---

## What's next?
### Postgression
Still to-do is actually doing any work with PostgreSQL. I'm comfortable enough in SQL itself to not be too worried, but it'd be good to test it out a bit, see if it works as expected.  
The database itself needs a schema, which will likely be the very next step. It doesn't need to be perfect, but it should at least handle what Trine brings in for her first batches of data.  


### Redis
I've also mentioned setting up a Redis database for caching timeblocks. This is still in consideration, but this is extremely low priority for now.  
Timeblocks will be 


### Application development when??
So far I've essentially neglected my second specialization. This is completely on purpose, because it doesn't ~~provide any value~~ block anyone else from progressing their work. The database, however, could potentially. So priorities were had, and Application Development has been the bottom of the list (of two subjects).  
