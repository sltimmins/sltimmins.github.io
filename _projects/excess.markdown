---
layout: page
title: Excess Construction Material Marketplace
permalink: /ecmm/
---

In this AGILE and team based course I worked in conjunction with 6 other teammates to design and develop an eBay style online marketplace for excess construction materials. The team was split between four backend developers and three front end developers. As a member of the backend team I designed and implemented a MySQL relational database and a Node.js API that was utilized by the frontend team.

The product included functionality such as user sign up and login, user reviews, fixed price and auction style listings, a notification system, as well as a transaction ledger. My role on the team was to setup our server using an AWS RDS as well as API and database design. I also implemented the MySQL tables that held user information, transactions, fixed price auctions, and products as well as the respective API routes for each table. For the user information table this included ensuring that user information and passwords were secure using SHA-256 encryption in the API calls. Below is the ERD I designed and used for this project.

<div style="text-align: center"><img src="/images/ExcessConstruction.png"  /></div>

The complete GitHub repository for this project can be found [here][excess-link].

[excess-link]: https://github.com/tmcnitt/GUI-Team2