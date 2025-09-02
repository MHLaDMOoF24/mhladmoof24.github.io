---
layout: post
title: "Database requirement profiling"
tags: [research, database]
---

## Hashing out the details
### Where we came from
Last post I put together a list of database types.  
Which is great and all, don't get me wrong, but to choose one, I need to understand the requirements of our project.  


### Currently suggested design
Within the group we've made a rough draft for the high-level design of our project.  
The following is the first sketch of the domain model.  
![Domain model]({{ "/assets/images/domain_diagram_1.png" | relative_url }})

From this we can somewhat understand the size and complexity of the data we need to handle.  
But what do we need to keep on file?  

**Timeblocks** (Date or Time-of-day)  
- Timeblocks are cached on search, but we don't need to store either type.  

**Services**  
- We want to be able to search by Service names.  
- Services can differ a lot from place to place, whether it's naming differences or offerings.  
- Likely we'll need to keep a list of all possible services caught by the latest webscrape for this.  

**Salons**  
- Location likely will be scraped alongside the services, same with website.  
- After the scrape, we'll want to store the information.  

A Salon will offer a certain range of Services.  
Each of these are a class per say, with their own sets of datapoints.  
A Salon will always have a Location or a Contact (website) (for our usecase, at least).  
A Service will always have a Name, a Duration, and a Price.  

_Reminder that this is just the currently suggested design, and likely will undergo further changes as the project progresses._  


### Additional notes
We don't like Microsoft. If we can avoid the company as a whole, we will.  
<span class="note">Many words could be said about this particular subject, but I won't get into it here. Story for another time.</span>  
Since we've already worked with T-SQL, for the sake of learning something new, a different 'dialect' would be required.  
<span class="note">Assuming we choose an SQL solution.</span>  
PO will provide no financial support for the development of this project.  
<span class="note">As such, we need a database solution that at least has a free version.</span>  
We have two hosted environments. One Windows, one Linux. Preferably we'd like to use the Linux host for our final version.  
For now, we're assuming the project will not see commercial viability, and won't build it towards such.  


### Known unknown variables
As it stands, we don't know how our data intake will look.  
Will geometrical data be from Google Maps, OpenMaps, or something else? How will the data look?  
In general, it seems like we'll be scraping data from each salon's website, but there's a chance we can get access to a subset of APIs. In either case, once again, how will this data look or be formatted?  
We've never even touched webscraping before, so that whole ballpark is a black box to us.  

---

## Comparing to database architectures
### Quick overview of requirements
- Primary data is strongly structured  
- Data intake is unknown, but expected messy  
- Data volume is not enormous  
- No Microsoft grr >:(  
- Free to use product  
- Data needs to be easily compared, to enable simple de-/activation  
- Potentially short-term caching of available timeblocks  


### When you're wielding a hammer...
With how structured the data is, and how we're sure that every entity will have all required fields, a relational database seems to be the best fit.  
The data volume is not expected to be enormous, so we don't need to worry about scaling or sharding.  
The data needs to be easily compared, which is a strong suit of relational databases.  
... And with all these nails, why not use a hammer?  

---

## PostgreSQL + Redis?
### A known SQuantityL
Because of previous experience with T-SQL and the SSMS environment, PostgreSQL will be quick to pick up, as per a few searches on the internet, the languages seem extremely similar.  
This is potentially an issue in terms of 'do I learn enough new stuff' in regards to my education, but an SQL variation seems to be the best fit for our project regardless.  
<span class="note">Not much of an issue, by simply expanding the subject a bit I'll be fine.</span>  


### Caching with Redis?
This is more of an unknown, both on the side of Redis (NoSQL) itself, and the datascraping that would inform the data structure itself.  
While I have a general gist of how Redis works, I've never used it in a project before. Something to be experimented with.  
Datascraping is not something I can even begin to guess at the ins and outs of, and it's not my subject to investigate.  