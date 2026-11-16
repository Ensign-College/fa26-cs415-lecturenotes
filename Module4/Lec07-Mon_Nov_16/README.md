CS415 - Monday, November 16 - Lecture 7 - Module 4

# Topics:
* [Opening Exercises](#opening-exercises)
* [Week 4 Overview and Mid-Term Preview](#week-4-overview-and-mid-term-preview)
* [Why Separate Servers](#why-separate-servers)
* [The AWS Console and Elastic IPs](#the-aws-console-and-elastic-ips)
* [A Database Server: EC2 and PostgreSQL](#a-database-server-ec2-and-postgresql)
* [Reaching Your Database From Home](#reaching-your-database-from-home)
* [Break](#break)
* [Work Time](#work-time)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Announcements

0.  I have graded all week 1 and week 2 submissions. I will work on your week 3 submissions soon.
    -   I have begun grading your attendance based on responses to the announcements made on lecture days.
    -   Your score on the **Roll Call Attendance** assignment is $`\frac{no. responses you made}{no. lectures that occurred since you enrolled}`$.
        -   You are not penalized for lectures missed because you enrolled late
    -   Everyone gets 1 freebie. If you need to miss more than one lecture you need to get it excused by me.
    -   If you forgot to respond to an announcement for a day that you were in class, those points are lost as I have no other way to verify your attendance.
0.  The [Mid-Term Assignment](https://ensign.instructure.com/courses/27288/modules/items/3250488) and [Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489) are due Saturday, November 21.
0.  You need **pgAdmin 4** today. If you did not install it in Week 2, download it from [pgadmin.org](https://www.pgadmin.org/download/) now.
0.  Start your AWS Academy lab now so it is ready when we need it.
    -   Contact me ASAP if you have any trouble logging in to AWS Academy


# Opening Exercises

### Song *Hymn: #220 [Lord, I Would Follow Thee](https://www.churchofjesuschrist.org/media/music/songs/lord-i-would-follow-thee)*

### Prayer

### Today's Spiritual Thought

This week your app stops living on one laptop: the database moves to its own server, and the API moves to its own container. Each part has one job, and each needs the others. Paul described the Church that Christ organized the same way, as one body with many members and many gifts. What part do you play in the body you belong to?


### Scripture

> 12. For as the body is one, and hath many members, and all the members of that one body, being many, are one body: so also is Christ.
>
> 21. And the eye cannot say unto the hand, I have no need of thee: nor again the head to the feet, I have no need of you.
>
> 26. And whether one member suffer, all the members suffer with it; or one member be honoured, all the members rejoice with it.
>
> -- [1 Corinthians 12:12,21,26](https://www.churchofjesuschrist.org/study/scriptures/nt/1-cor/12?lang=eng&id=p12,p21,p26#p12)


### Discussion Questions

0.  Why might a database be safer and easier to manage on its own server? What does Paul's picture suggest about why the Lord gives people different roles?
0.  Verse 26 says that when one member suffers, all suffer. What happens to your API when your database server stops? Who in this room could you help this week?
0.  What gift do you bring to a team that others may not have?

[Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489) asks how organizing a technical project is like the way Christ organized His ministry to heal the world. Today's discussion is a good place to start.


# Week 4 Overview and Mid-Term Preview

This course is **incremental**. Each week builds on the last, and nothing you build is thrown away. This week the same database and the same API leave your laptop.

## Where you are

In Weeks 2 and 3 you ran PostgreSQL in a container on your laptop and built an API with `GET`, `POST`, and `PATCH` endpoints and Swagger documentation.

## This week

0.  Move your database to a virtual machine in AWS, where it stays for the rest of the course
0.  Connect pgAdmin to it from your laptop
0.  Point your API at the database in AWS
0.  Run your API in its own Docker container

Read these in order as you work. Each one depends on the one before it, so do not skip steps:

0.  [Elastic IP Addresses](https://ensign.instructure.com/courses/27288/modules/items/3250483)
0.  [Deploying the Database to AWS](https://ensign.instructure.com/courses/27288/modules/items/3250484)
0.  [Connect the API to a Remote Database](https://ensign.instructure.com/courses/27288/modules/items/3250485)
0.  [Running the API in a Container](https://ensign.instructure.com/courses/27288/modules/items/3250486)


*   Ponder: [Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489)
*   Prove: [Mid-Term Assignment](https://ensign.instructure.com/courses/27288/modules/items/3250488)

Both are due Saturday, November 21.

## The Mid-Term

The Mid-Term is worth 50 points. You upload 7 screenshots that show your database in AWS and your API in a container. The assignment shows an example of each one, and you can take the first 3 as soon as your database is in AWS.

Moving the database does not depend on your Week 3 endpoints, so everyone can do it. You are the agent of your own learning, and class time is for working while I help.

*Check:* Which of your two projects, `cs415_db` or `cs415_api`, do you need to move your database, and which to run your API in a container?


# Why Separate Servers?

Until now, your whole app ran on your laptop. A real application puts its parts on separate servers:

*   The **front end**, the website people see
*   The **API**, which answers requests from the front end
*   The **database**, which keeps the data

This week your database moves to a server in AWS. Your API stays on your laptop, in a container, and reaches the database across the internet.

## Why split them up?

*   **The data needs a stable home.** Laptops close and containers get replaced. A database server keeps running.
*   **Each part grows on its own.** Many API servers can share one database.
*   **Each part fails and updates on its own.** You can restart or replace the API without touching the data.
*   **Security.** Users should never reach the database directly. Only the API should.
*   **Different needs.** A database needs disk and memory. An API needs processing power. A front end mostly sends files.


## What it costs

More servers means more moving parts. The network between them brings new ways to fail: the wrong IP address, a closed firewall port, a wrong password. You will meet all of these this week, and learning to find them is part of the skill.

## Explain it

Turn to a neighbor. In your own words, explain why your database is leaving your laptop. Then switch.

*Check:* Your API runs on your laptop and your database runs in Oregon. Name two things that must be true for the API to read one row.


# The AWS Console and Elastic IPs

Your AWS Academy lab starts and stops each time you use it. When a server in AWS stops and starts, it usually gets a new public IP address. Anything that pointed at the old address, like pgAdmin or your API, breaks.

An **Elastic IP** is an address that belongs to you and stays the same. You attach it to a server, and the server keeps that address every time it starts.

Follow [Elastic IP Addresses](https://ensign.instructure.com/courses/27288/modules/items/3250483).

## Find your way around

0.  Start your lab. For help, see [AWS Academy, AWS CLI - Overview and Setup](https://ensign.instructure.com/courses/27288/pages/aws-academy-aws-cli-overview-and-setup)
0.  Click the **AWS** link in the upper left corner
0.  Set the region in the upper right to **us-west-2 (Oregon)**. If your server ever seems to disappear, check the region first
0.  Open **EC2**. The menu on the left has **Instances**, **Elastic IPs**, **Security Groups**, and **Key Pairs**


## Demo and your turn

0.  Click **Elastic IPs**, then **Allocate Elastic IP address**
0.  Choose Amazon's pool of IPv4 addresses and the network border group `us-west-2`, then click **Allocate**
0.  Click the pencil icon on the new address and name it `cs415_db_ip`
0.  Allocate a second address the same way and name it `cs415_api_ip`. You will use it in a later week


In a real AWS account, public IP addresses cost money by the hour, even when they are not attached to anything. Release the ones you do not need.

*Check:* Your lab session ends tonight and starts again tomorrow. Without an Elastic IP, what breaks, and where would you have to fix it?


# A Database Server: EC2 and PostgreSQL

**EC2** (Elastic Compute Cloud) gives you virtual machines in AWS. You will create one running Ubuntu Linux and install PostgreSQL on it.

Follow [Deploying the Database to AWS](https://ensign.instructure.com/courses/27288/modules/items/3250484).

## Launch the instance

0.  In EC2, click **Launch instance**
0.  Name it `cs415_postgres`
0.  Choose the **Ubuntu Server** image and the **t2.micro** instance type
0.  Click **Create new key pair**, name it `cs415_key`, choose `.pem`, and create it. Keep the file it downloads
0.  Under **Network settings**, click **Edit**, then **Add security group rule**. Choose type **PostgreSQL** and source type **Anywhere**. Leave the SSH rule in place; the browser terminal needs it
0.  Click **Launch instance**
0.  On the **Instances** page, wait until the state is **Running**. Select it, click **Connect**, choose **EC2 Instance Connect**, and click **Connect**


A terminal opens in a new tab. When you paste into it, right-click and choose **Paste as plain text**.

## Install and configure PostgreSQL

0.  `sudo apt-get update`
0.  `sudo apt install postgresql -y`
0.  `ls /etc/postgresql` shows your PostgreSQL version number. The page uses `16`. If yours is different, use your number in the next two paths
0.  `sudo nano /etc/postgresql/16/main/postgresql.conf`. Find the line `#listen_addresses = 'localhost'`, remove the `#`, and change it to `listen_addresses = '*'`. Save with Ctrl+X, then Y, then Enter
0.  `sudo nano /etc/postgresql/16/main/pg_hba.conf`. Under `# IPv4 local connections:`, add this line, using Tab between the values:
    ```
    host    all    all    0.0.0.0/0    md5
    ```
0.  `sudo systemctl restart postgresql.service`
0.  `sudo -u postgres psql`
0.  Paste the three lines from your `cs415_db` project's `1_create_database.sql`, one at a time
0.  Type `\q` to leave `psql`


## Three gates

A computer outside AWS must pass three gates to reach your database. All three must say yes:

| Gate | Where it lives | What it decides |
| ---- | -------------- | --------------- |
| Security group | AWS, outside your server | Which ports are open to the internet |
| `listen_addresses` | `postgresql.conf` | Which network connections PostgreSQL listens on |
| `pg_hba.conf` | PostgreSQL's own settings | Who may log in, from where, and how |

Two commands on your server help you check them:

*   `pg_lsclusters` shows whether PostgreSQL is really running. Its Status column should say `online`
*   `sudo ss -tlnp | grep 5432` shows what PostgreSQL listens on. `0.0.0.0:5432` means every address. `127.0.0.1:5432` means only the server itself

*Check:* pgAdmin cannot reach your database. One classmate sees "timeout," another sees "connection refused," and a third sees "no pg_hba.conf entry." Which gate is closed for each?


# Reaching Your Database From Home

Your database is running in AWS. Now give it a permanent address and connect to it from your laptop.

Follow the last three sections of [Deploying the Database to AWS](https://ensign.instructure.com/courses/27288/modules/items/3250484).

## Attach the Elastic IP

Do this before you copy the address anywhere.

0.  In EC2, click **Elastic IPs** and select `cs415_db_ip`
0.  Click **Associate Elastic IP address**
0.  Choose your `cs415_postgres` instance and click **Associate**
0.  On the **Instances** page, the instance's **Public IPv4 address** now matches `cs415_db_ip`


## Connect pgAdmin

0.  In pgAdmin, right-click **Servers**, then choose **Register**, then **Server**
0.  Name it `AWS - cs415`
0.  On the **Connection** tab, paste your Elastic IP into **Host name/address**
0.  Enter the user name and password from your `1_create_database.sql`, turn on **Save password**, and click **Save**
0.  Expand the server. You should see the `cs415` database, with no tables yet


## Create the tables and data

0.  Click the `cs415` database, then open the **Query Tool**
0.  Paste everything from `2_create_tables.sql` except the first line, `\c cs415`. Click **Execute script**
0.  Refresh **Tables**. You should see all 7
0.  Clear the query area. Paste `3_insert_data.sql`, again without `\c cs415`, and execute it
0.  Look at the data in `webuser`


`\c cs415` is a `psql` command, not SQL. pgAdmin does not understand it, and you are already connected to `cs415` because you opened the Query Tool on it.

## How safe is this?

Your database now accepts connections from the whole internet, on a well-known port that attackers scan for all day. Everyone in the class uses the same password, and the account can do anything, including run commands on the server. Attackers have taken over more than a thousand database servers set up like this.

For a class lab, we accept that risk to keep things simple. What would you change before real users trusted this server with their data?

*Check:* Why must you remove `\c cs415` before you run a script in pgAdmin?


# Break

Take 10 minutes.


# Work Time

Use the rest of class on your assignments while I help:

0.  Finish [Deploying the Database to AWS](https://ensign.instructure.com/courses/27288/modules/items/3250484): your instance is running, and pgAdmin shows all 7 tables with data
0.  Take [Mid-Term Assignment](https://ensign.instructure.com/courses/27288/modules/items/3250488) screenshots 1 to 3: your instance in a **Running** state, pgAdmin's **Connection** tab for `AWS - cs415`, and the data in `webuser`
0.  Start your [Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489)
0.  If you finish, start [Connect the API to a Remote Database](https://ensign.instructure.com/courses/27288/modules/items/3250485)


Push your work to GitHub as you go. If you finish early, help a classmate who is stuck.


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module4/Lec07-Mon_Nov_16
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement


## Exit Ticket Questions

Pick **one** to answer in your Canvas reply:

0.  A computer outside AWS must pass three gates to reach your database. Name them and say what each one controls.
0.  How far did you get today: instance running, PostgreSQL configured, pgAdmin connected, tables and data loaded?
0.  What is one thing about EC2, Elastic IPs, or the PostgreSQL setup that you are still unsure about?


# Wrap Up & Parting Thoughts

## Before the next meeting

Your API connects to this database next time, so have these done before we meet again:

0.  Your `cs415_postgres` instance has `cs415_db_ip` attached
0.  pgAdmin connects to `AWS - cs415` and shows all 7 tables with data


## Keep your server

Your instance stops when your lab session ends and starts again with the lab. You may also stop it yourself. Never **terminate** it: terminating deletes the server and your database with it.

## Also this week

0.  Take Mid-Term screenshots 1 to 3 while everything is open
0.  Read the talk and scripture for [Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489), then answer all four questions with specific examples from your own work. The Ponder and Respond assignments together are about a quarter of your course grade
0.  Watch Canvas for updates, and check due dates there


## What is next

Next meeting you will point your API at the database in AWS and run the API in its own container.


