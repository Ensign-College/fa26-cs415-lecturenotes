CS415 - Monday, October 26 - Lecture 1 - Module 1

# Topics:
* [Today's Spiritual Thought](#todays-spiritual-thought)
* [Course Overview](#course-overview)
* [Get To Know Your Professor](#get-to-know-your-professor)
* [Get To Know Each Other](#get-to-know-each-other)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [GitHub](#github)
* [Databases and Schemas](#databases-and-schemas)
* [Break](#break)
* [Work Time: Website Database Schema Diagram](#work-time-website-database-schema-diagram)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Action Items

0.  Prayer
0.  Spiritual Thought
0.  Course Overview
0.  GitHub
0.  Databases and Schemas
0.  Break
0.  Work Time: Website Database Schema Diagram
0.  Exit Ticket
0.  Wrap Up & Parting Thoughts


# Today's Spiritual Thought

This course is built one week on top of the next, and so is discipleship. Small, steady steps compound into momentum, and a weak foundation makes every later step harder. A container image is built in layers, each resting on the one beneath it. What happens to the whole build when an early layer is wrong?


## Scripture

> Wherefore, be not weary in well-doing, for ye are laying the foundation of a great work. And out of small things proceedeth that which is great.
>
> -- [Doctrine and Covenants 64:33](https://www.churchofjesuschrist.org/study/scriptures/dc-testament/dc/64?lang=eng&id=p33#p33)


## Discuss

0.  Where have you seen small, steady habits create momentum, in school, work, or spiritual life?
0.  What "foundation" do you most want to lay this block, technical or spiritual?
0.  How can we help each other keep going when the work gets hard?


# Course Overview

This is an **incremental, project-based** class. Each week builds on the one before it, so the work you do this week is the foundation for everything that follows.

## How each week is organized

*   **Preparation & Resources**: read these *before* you start the assignments, and refer back to them as you work
*   **Ponder**: a short written response about a spiritual principle
*   **Prove**: the hands-on assignments that show you can do the work

## This week

*   GitHub: create repositories and connect them using VS Code
*   AWS Academy and the AWS CLI: overview and setup
*   Database overview and schema
*   Containers: Docker Desktop, running a container, Docker Compose

Take Week 1 seriously or you will struggle with Week 2 and beyond.


# Get To Know Your Professor

Hi, I'm Brother Erik Falor, your professor for this course.

I was fascinated by computers from a young age, but it wasn't until I was in eighth grade that my family got our first computer. In High School I considered myself to be "good with computers," but I didn't begin programming until I was 21. I did not fit in with my peers who could truly call themselves "computer nerds": I am bad at math and I don't particularly enjoy *Star Trek*, *Dungeons and Dragons*, or *Magic: The Gathering*.

I started college as a Communications major but shifted to Computer Science after serving in the Taiwan Taichung mission. My late start meant that I had to play catch-up with classmates who had been programming from a very young age. This is how I learned I wasn't as good at computers as I thought. I remember being intimidated by my classmates who had programmers for parents, went to programming summer camps, made their own video games, and generally understood the jargon words my professor spoke. It felt so humiliating to be shown up by kids who were 2 or 3 years younger than me!

I stuck with the program and earned my Bachelor's degree. I went on to spend 13 years as a software engineer at Spillman Technologies, Inc. (now Motorola Solutions) and Automated Products Group. Midway through my career I began studying for my Master's Degree part-time at USU, graduating in fall 2016. I first taught as an adjunct there in spring 2017 and have been a teacher ever since.

When I was a software engineer I spent my days:

*   _Reading_ horrible documentation
*   _Writing_ documentation that was, hopefully, an improvement
*   _Maintaining_ software that was first written **way** before my time.  Some of the oldest code I encountered was first written when I was still in elementary school in the 90's.
    *   _Testing_ my code
    *   _Finding_ and fixing bugs
    *   _Monitoring_ our customers' systems
    *   _Porting_ programs between platforms
    *   _Reviewing_ code written by my team
*   _Building_ large-scale software systems (millions of lines of code)
*   _Designing_ new software
    *   _Writing_ brand-new code in Perl, C, C++, Java/C#, XSLT, and Shell with tools such as Vim, VisualStudio, Eclipse, and IAR Embedded Workbench

The lessons I learned from my own Algorithms & Data Structures course informed my programming decisions on a near daily basis.  This course was one of the most impactful courses I took, and I am excited to guide you on your journey through it!


# Get To Know Each Other

Take 45 seconds to introduce yourself

0.  Name
0.  Where you are from
0.  Why you are taking this class
0.  Your favorite programming language
0.  One interesting non-CS fact about you (talent, hobby, etc.)


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled **Module1/Lec01-Mon_Oct_26**
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement


# GitHub

<details>
<summary>What do you think it is?</summary>

*   A website that hosts **Git** repositories
*   Git records the history of your project: who changed what, and when
*   You can undo mistakes, work with others, and keep a backup off your computer
*   GitHub also hosts cloud computing resources that work with your repositories
    -   Automated code testing and packaging
    -   Codespaces for editing code through your web browser

You will use GitHub for the whole course. Your work lives in repositories, and each week's project builds on the last.

</details>

## Live demo

0.  Create a repository on GitHub
0.  Clone it in VS Code
0.  Make a change to a file
0.  **Commit** the change (record it locally)
0.  **Push** the commit (send it to GitHub)


## Your turn

Create your own repository and connect it in VS Code. Follow the Canvas page [GitHub - Create Repositories and Connect Using VSCode](https://ensign.instructure.com/courses/27288/pages/github-create-repositories-and-connect-using-vscode).


*Check:* what is the difference between a commit and a push?


# Databases and Schemas

On Canvas: [Database Overview and Schema](https://ensign.instructure.com/courses/27288/pages/database-overview-and-schema?module_item_id=3250446)


## Relational database vocabulary

*   **Table**: one kind of thing (students, courses, orders)
*   **Row**: one item in a table
*   **Primary key**: the column that uniquely identifies a row
*   **Foreign key**: a column that points at a row in another table

## Relationships

*   One-to-one
*   One-to-many
*   Many-to-many (needs a join table)


## Entity Relationship Diagrams (ERDs)

An ERD is a picture of a **schema**: the tables, their columns, and the lines that document how tables relate to each other.


## On the board

Sketch a small example together (students and courses). Which relationship is it? What would the foreign keys be?


# Break

Take 5 minutes.


# Work Time: Website Database Schema Diagram

Start the assignment now while I am here to help: [Website Database Schema Diagram](https://ensign.instructure.com/courses/27288/assignments/1629931)


## Goals for today

0.  Pick out the tables your website needs
0.  Mark the primary and foreign keys
0.  Draw the relationship lines and decide which kind each one is
0.  Push your work to your GitHub repository

If you finish early, help a classmate who is stuck.


# Wrap Up & Parting Thoughts

**Before we meet again**

0.  Respond to today's announcement in Canvas to fill out your exit ticket 
0.  Install Docker Desktop
    -   Instructions are on Canvas [Containers - Install Docker Desktop](https://ensign.instructure.com/courses/27288/pages/containers-install-docker-desktop)
0.  Keep working on the Website Database Schema Diagram
0.  Complete [Ponder and Respond #1: The Power of Spiritual Momentum](https://ensign.instructure.com/courses/27288/assignments/1629921)


