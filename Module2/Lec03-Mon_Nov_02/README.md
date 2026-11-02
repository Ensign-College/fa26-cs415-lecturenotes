CS415 - Monday, November 02 - Lecture 3 - Module 2

# Topics:
* [Today's Spiritual Thought](#todays-spiritual-thought)
* [Week 2 Overview and Q&A](#week-2-overview-and-qa)
* [What is an Application Programming Interface (API)](#what-is-an-application-programming-interface-api)
* [Postgres in a Container: Dockerfile and Compose](#postgres-in-a-container-dockerfile-and-compose)
* [Break](#break)
* [SQL Init Scripts: Database, Tables, and Seed Data](#sql-init-scripts-database-tables-and-seed-data)
* [Checking the Data with `psql`](#checking-the-data-with-psql)
* [Work Time](#work-time)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Action Items

0.  Song *Hymn: #85 [How Firm a Foundation](https://www.churchofjesuschrist.org/media/music/songs/how-firm-a-foundation)*
0.  Prayer
0.  Spiritual Thought
0.  Week 2 Overview and Q&A
0.  What Is an API, and Why Does Organized Data Matter?
0.  Postgres in a Container: Dockerfile and Compose
0.  Break
0.  SQL Init Scripts: Database, Tables, and Seed Data
0.  Checking the Data with psql
0.  Work Time
0.  Exit Ticket
0.  Wrap Up & Parting Thoughts


# Today's Spiritual Thought

> Jesus Christ the same yesterday, and to day, and for ever.
>
> -- [Hebrews 13:8](https://www.churchofjesuschrist.org/study/scriptures/nt/heb/13?lang=eng&id=p8#p8)

> And ye may know that he is, [...] the same today and tomorrow, and forever.
>
> -- [Moroni 10:7](https://www.churchofjesuschrist.org/study/scriptures/bofm/moro/10?lang=eng&id=p7#p7)

> For do we not read that God is the same yesterday, today, and forever, and in
> him there is no variableness neither shadow of changing?
>
> -- [Mormon 9:9](https://www.churchofjesuschrist.org/study/scriptures/bofm/morm/9?lang=eng&id=p9#p9)

> **11** Proving to the world that the holy scriptures are true, and that God does
> inspire men and call them to his holy work in this age and generation, as
> well as in generations of old;
>
> **12** Thereby showing that he is the same God yesterday, today, and forever. Amen.
>
> -- [D&C 20:11-12](https://www.churchofjesuschrist.org/study/scriptures/dc-testament/dc/20?lang=eng&id=p11-p12#p11)

A Docker container can be destroyed and rebuilt in seconds, and it comes back exactly the same way each time because the instructions that describe it never change. Engineers depend on this feature to increase the reliability of the systems they build. Only by building layer upon dependable layer can they have any assurance that their system will remain functional throughout its lifecycle.

In our day-to-day lives we can likewise rely on the Savior. While our circumstances may change, He does not. In what changing circumstances has the Savior been there for you?


# Week 2 Overview and Q&A

This course is **incremental**. Each week builds on the last, and nothing you build this week is thrown away.

## What you built in Week 1

You have GitHub repositories (`cs415_db` and `cs415_api`), an ERD for your database schema, and Docker Desktop. This week turns that ERD into a running database, and then puts an API on top of it.

## This week

0.  **Today:** run PostgreSQL in a container on your own computer and look at its data.
0.  **Next meeting:** build a Django REST Framework (DRF) project that reads that database and serves it as JSON.

Each week has three parts on Canvas: **Preparation & Resources** (read first), **Ponder**, and **Prove**.

0.  Read these pages as you work: [PostgreSQL Database](https://ensign.instructure.com/courses/27288/pages/postgresql-database) and [Accessing the Data](https://ensign.instructure.com/courses/27288/pages/accessing-the-data)
0.  Ponder: [Ponder and Respond #2 - Connecting Principles](https://ensign.instructure.com/courses/27288/assignments/1629922)
0.  Prove: [Pulling API Data](https://ensign.instructure.com/courses/27288/assignments/1629928)


You are the agent of your own learning. Most of class is time for you to work while I help, so follow the Canvas tutorials in order.

## Questions from last week

Before we start: is Docker Desktop installed and running on your computer? Do you have both repositories and your ERD? Ask now so we can fix it before the work starts.

*Check:* What did you build in Week 1 that you will use again today?


# What is an Application Programming Interface (API)?

The traditional definition of API referred to libraries of code used in a programming environment.  Historically, an API included:

*   Packages you can import/include into a program
*   Variables that are defined (global or within a namespace)
*   Functions (and methods) that can be called
*   Data types taken as parameters and returned as values
*   Classes and objects that can be instantiated

You can think of an API as a *menu of offerings* that one program serves to another. To take this metaphor in the direction of a restaurant, a *client* orders something from the menu and gets their expected order without needing to know how it was prepared in the kitchen.

More recently, the concept of API covers protocols used to facilitate communication between systems. Nowadays when you say "API," most people will think of a "web API."

A web API might describe:

*   The URLs that data may be retrieved from or sent to
*   What format that data will be presented in (i.e. XML, JSON, binary, etc.)
*   How a user can authenticate themselves to the system, and how many system resources they are allowed to consume

In this course you'll study both kinds of APIs.


## How a web API works

0.  A client (a browser, Postman, or a web page) sends an **HTTP request** to a URL
0.  The server on the receiving end calls a function or program that handle that particular URL
0.  The code gets the data it needs from a database
0.  Finally, the server sends back an **HTTP response** with the clients requested information
    -   Often this response takes the form of **JSON**, a plain-text format for structured data
    -   However, any kind of appropriate data can be sent to the client (binary data for images, sounds, or movies; plain text or Markdown; structured text like XML, etc.)


## Why organized data matters

An API can only return data as well as it is stored. In your ERD, a `WebUser` has many addresses. Because addresses live in their own `UserAddress` table linked by a key, an API can return one user, one address, or all of them without repeating itself.

If everything lived in one messy table, every answer would be harder to find, easier to get wrong, and slower to change. Often, excessive data would be sent back to the client, wasting bandwidth and putting the burden of sorting it out on the client.


## Where this is going

By the end of our next meeting, you will open a URL in a browser and see rows from your own database as JSON. Today you build the database it will read from.

*Check:* If all user and address data lived in one messy table, what would get harder for an API?


# Postgres in a Container: Dockerfile and Compose

You will run **PostgreSQL** (also called Postgres) in a container on your own computer. Follow the [PostgreSQL Database](https://ensign.instructure.com/courses/27288/pages/postgresql-database) page step by step. If Docker terms are new to you, review [Running a Container](https://ensign.instructure.com/courses/27288/pages/running-a-container) and [Docker Compose](https://ensign.instructure.com/courses/27288/pages/docker-compose) from last week.

## Demo: the Dockerfile

In your `cs415_db` project in VS Code, make a folder named `create`, then a file named `Dockerfile`. It has four instructions:

0.  `FROM` picks the official Postgres image to start from
0.  `ENV` sets the password the database will use
0.  `COPY` puts the contents of your `create` folder into the folder where Postgres looks for start-up scripts
0.  `EXPOSE` declares the Postgres port, 5432

## Demo: docker-compose.yml

Next, a `docker-compose.yml` file describes the **service** (the container). It names the service, tells Compose to build from your Dockerfile, names the image and the container, restarts the container if it fails, and forwards port `5432:5432` from the container to your computer.

Indentation matters in YAML. Copy the layout from the page exactly.

## Your turn

0.  Create the `create` folder, the Dockerfile, and the compose file
0.  Save both files
0.  Do not run `docker compose up -d` yet. You run it in the next part, once the `create` folder has its SQL scripts

*Check:* What does `5432:5432` mean, and which side is your computer?


# Break

Take 10 minutes.


# SQL Init Scripts: Database, Tables, and Seed Data

A container is short-lived. Each time one is created, it needs instructions for its starting state. For a Postgres container, those instructions are SQL scripts.

## How the scripts run

Your Dockerfile copies your `create` folder into `docker-entrypoint-initdb.d`. When a new container starts, Postgres runs every script in that folder in alphabetical order. That is why the files are numbered. The scripts run only when the container is created, not when you stop and start it.

## The three scripts

0.  `1_create_database.sql` creates the `cs415` database and a user, and grants that user access
0.  `2_create_tables.sql` connects to `cs415` (the line `\c cs415`) and creates your tables
0.  `3_insert_data.sql` connects to `cs415` and adds seed data so you have something to test with

You will end with 7 tables. Use the ERD on the [PostgreSQL Database](https://ensign.instructure.com/courses/27288/pages/postgresql-database) page, and your own ERD from Week 1, as your guide.

## Demo

We will write the first script, then the `WebUser` table and its rows, and then build the container:

0.  `docker compose up -d` builds the image and starts the container in the background
0.  `docker compose down --rmi all` stops and deletes the container and image, so you can rebuild after changing a script

## Your turn

0.  Write `1_create_database.sql`
0.  Write the `WebUser` table in `2_create_tables.sql` and its rows in `3_insert_data.sql`
0.  Build the container and check in Docker Desktop that its status is Running and its port is `5432:5432`
0.  If it fails, open the container's logs in Docker Desktop, find where the first error happens, fix your script, and rebuild

*Check:* Why do we destroy and rebuild the container after editing a script, instead of just restarting it?


# Checking the Data with `psql`

You can look inside your database from the command line with `psql`, the Postgres interactive prompt. The full walkthrough is on the [Accessing the Data](https://ensign.instructure.com/courses/27288/pages/accessing-the-data) page.

## Demo

0.  Make sure your container is running
0.  Open a shell in it: `docker exec -it container_cs415_db bash`
0.  Start the Postgres prompt: `psql -U postgres`
0.  List the databases: `\l` (backslash, lowercase L). Find `cs415`
0.  List the users: `\du`. Find the user your script created
0.  Connect to your database: `\c cs415`
0.  List the tables: `\dt`
0.  Describe one table: `\d webuser`
0.  Look at its rows: `select * from webuser;`

SQL commands end with a semicolon. Commands that start with a backslash do not.

## Your turn

Run each command against your own container and compare what you see to your scripts. Is everything there that you wrote?

pgAdmin is an optional visual tool for the same job. It is described at the bottom of the [Accessing the Data](https://ensign.instructure.com/courses/27288/pages/accessing-the-data) page.

*Check:* How can you tell that your init scripts actually ran?


# Work Time

Use the rest of class on your assignments while I help:

0.  Get your container running with port `5432`, following the [PostgreSQL Database](https://ensign.instructure.com/courses/27288/pages/postgresql-database) page
0.  Finish your [seed data for the remaining tables](https://ensign.instructure.com/courses/27288/assignments/1629928)


Push your work to GitHub as you go. If you finish early, help a classmate who is stuck.


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module2/Lec03-Mon_Nov_02
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement


## Exit Ticket Questions

Pick **one** to answer in your Canvas reply:

0.  Describe how you check that your Postgres container is running and which port it uses. What did you see?
0.  What do your SQL scripts in the `create` folder contain so far (which tables, which seed data)?
0.  What is one thing about Docker or the database that you are still unsure about?


# Wrap Up & Parting Thoughts

## Before the next meeting

These must be done before we meet again, because the API builds directly on them:

0.  Finish `2_create_tables.sql` and `3_insert_data.sql` so your container shows all 7 tables with data
0.  Install Python (version 3.9, 3.10, or 3.11; 3.11 is recommended) and check it by typing `python --version` in a terminal. If you get an error, try `python3 --version`
0.  Have your `cs415_api` repository cloned and open in VS Code

Instructions for installing Python are on the [Django - Setup and Run](https://ensign.instructure.com/courses/27288/pages/django-setup-and-run) page.

## Also this week

0.  Start your reflection for [Ponder and Respond #2 - Connecting Principles](https://ensign.instructure.com/courses/27288/assignments/1629922). The Ponder and Respond assignments together are about a quarter of your course grade, so give it real thought
0.  Read the Prove assignment, [Pulling API Data](https://ensign.instructure.com/courses/27288/assignments/1629928), so you know where we are headed
0.  Watch Canvas for updates, and check due dates there

## What is next

Next meeting you will create a Django REST Framework project, connect it to this database, and build your first API endpoint.


