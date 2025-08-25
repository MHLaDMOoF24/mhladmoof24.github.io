---
layout: post
title: "Initial database research"
tags: [research, database]
---

## Baby steps
### A breadth of choice
<span class="note">Initial research at <a href="https://www.geeksforgeeks.org/dbms/types-of-databases/">GeeksforGeeks</a>.</span>  
What have I gotten myself into? Choosing a category of a category? Hopefully this rabbit hole isn't any deeper.  
From what I've gathered in a few minutes of searching, I have six choices for my main category.  
1. Relational (SQL)  
2. Non-relational (Document, Key-Value, Graph, Column-Family) (NoSQL)  
3. Object-oriented (Attributes and methods)
4. Hierarchical (Tree model)  
5. Network (Graph model)  
6. Cloud-Based (It's in the cloud) (Can this even be considered a database type??)  


### Some questions
So that list caused some questions.  
Cloud-based? Is that not a hosting solution? Why is that relevant to the type of database?  

Once you get into the meat of the article, a total nine types of databases are mentioned.  
Those not initially listed were as follows:  
1. Centralized (Single location)  
2. Personal (Small-scale)  
3. Operational (Real-time processing)  

While I can see the relevance, I don't see how they would be compared to the architectural types, like SQL.  
Instead I'd put them into two, maybe three, categories.  
**Architectural:** Relational, Non-relational, Object-oriented, Hierarchical, Network  
**Scale:** Cloud-Based, Centralized, Personal  
**Purpose:** Operational  

### Exploring the categories
As a couple of examples of what these might be, a Personal database could be used on a phone to house contacts.  
An Operational database could be used in a bank to handle transactions.  
A Network database could be used to map out social connections.  
These are all functional examples, but the underlying architecture could be completely different.  

Alternatively, we could have a **"Relational" "Centralized" "Data Warehouse"** (unmentioned, but I'd label this as a Purpose).  
This would be touching all three categories, and fairly well nail down both purpose and functionality, without even touching on whatever solution it might be part of.  
