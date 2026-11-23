CS415 - Monday, November 23 - Lecture 9 - Module 5

# Topics:
* [Opening Exercises](#opening-exercises)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [Why Are We Learning This](#why-are-we-learning-this)
* [Week 5: Your API in the Cloud](#week-5-your-api-in-the-cloud)
* [Push Your Image to ECR](#push-your-image-to-ecr)
* [Break](#break)
* [Work Time](#work-time)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Announcements

0.  There is no class this Wednesday, November 25. Happy Thanksgiving! We meet again Monday, November 30.
0.  [Show API Running in ECS](https://ensign.instructure.com/courses/27288/modules/items/3250498) and [Ponder and Respond #5 - Building Faith and Infrastructure](https://ensign.instructure.com/courses/27288/assignments/1629925) are due this Saturday, November 28.
0.  Start your AWS Academy lab and open Docker Desktop now. Both take a few minutes to be ready.
0.  Run `aws sts get-caller-identity` now. If it fails, raise your hand. See the announcement **Check your AWS CLI before Monday's class** and [AWS Academy, AWS CLI - Overview and Setup](https://ensign.instructure.com/courses/27288/modules/items/3250445).


# Opening Exercises

### Song *Hymn: #241 [Count Your Blessings](https://www.churchofjesuschrist.org/media/music/songs/count-your-blessings)*

### Prayer

### Today's Spiritual Thought

This week you hand your container to Fargate, which runs it on servers you will never see or log in to. You can trust it because you tested the very same image on your own laptop first. Proverbs asks us to trust the Lord with all our heart, and that trust grows the same way: from experience, one tested step at a time. As Thanksgiving comes, where can you see that the Lord has directed your paths?


### Quote

> Your exercise of faith in true principles builds character; fortified character expands your capacity to exercise more faith. As a result, your capacity and confidence to conquer the trials of life are enhanced. The more your character is fortified, the more enabled you are to benefit from exercising the power of faith. You will discover how faith and character interact to strengthen one another. **Character is woven patiently from threads of applied principle, doctrine, and obedience.**
>
> -- Elder Richard G. Scott, ["The Transforming Power of Faith and Character"](https://www.churchofjesuschrist.org/study/general-conference/2010/10/the-transforming-power-of-faith-and-character?lang=eng)


### Discussion Questions

0.  Fargate runs your container on a machine you will never see. What makes you willing to trust it? What has made you willing to trust the Lord when you could not see the whole path?
0.  When has following steps you did not fully understand yet brought you somewhere you could not have reached on your own?
0.  Elder Scott says character is woven *patiently*. What small, steady habit could you weave into this week, in your project or in your discipleship?
0.  Who has helped you get this far in this course or in your schooling? How could you thank them this week?

[Ponder and Respond #5 - Building Faith and Infrastructure](https://ensign.instructure.com/courses/27288/assignments/1629925) asks how each of this week's four AWS steps is like building faith and character. It uses this same scripture and talk. Answer each question as you finish that step, while the comparison is fresh.


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module5/Lec09-Mon_Nov_23
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement


## Exit Ticket Questions

Pick **one** to answer in your Canvas reply:

0.  In one sentence each, what do ECR, the Network Load Balancer, and ECS Fargate do in your project?
0.  How far did you get today: repository created, image pushed, load balancer created, service running?
0.  What is one thing about this week's AWS steps that you are still unsure about?


# Why Are We Learning This?

Last week your database moved to a server in AWS, and your API moved into a container. This week that container leaves your laptop too. Before we build on Week 4, let's see what stuck.

Talk these over with a neighbor first. Then I will call on people. Answer in your own words; there is no penalty for a wrong answer, and a wrong answer now is cheaper than one on Saturday.

0.  Why did we move the database off your laptop and onto its own server? What did we gain, and what did it cost?
0.  What is an **Elastic IP**, and what would break in your project without one?
0.  A **security group**, `listen_addresses`, and `pg_hba.conf` all decide who can reach your database. How would you explain the difference to someone who has never run a server?
0.  Explain the difference between a Docker **image** and a Docker **container** to a friend who has never used Docker.
0.  Why does your API now run in a container with Apache, instead of with `python manage.py runserver`?
0.  Your `docker-compose.yml` maps ports `80:80`. What do the two numbers mean? Trace a request to `http://localhost/users` from your browser to your database.
0.  Your container is running. `/swagger` loads, but `/users` returns a 500 error. How would you troubleshoot it? Where do you look first?
0.  What is the difference between **stopping** and **terminating** your EC2 instance? Why should you care?


*Check:* Which of these would you still struggle to explain? That is the one to ask me about during work time.


# Week 5: Your API in the Cloud

This course is **incremental**. Nothing is rebuilt this week. The image you built and tested in Week 4 is the one that runs in AWS.

## Why containers make this easier

*   **Same everywhere.** The exact image you tested on your laptop, operating system and all, is what runs in the cloud.
*   **Fewer surprises.** Because the OS travels with your code, moving to a new machine brings fewer compatibility problems.
*   **Scales on demand.** You can start more copies of a container when traffic grows and stop them when it falls.


## The big picture

```
Build:    your laptop  --docker push-->  ECR  --pulled by-->  ECS

Request:  browser --> Network Load Balancer --> Fargate task --> PostgreSQL on EC2
                      (port 80)                 (your container)  (cs415_db_ip, port 5432)
```

*   **ECR** (Elastic Container Registry) stores your image in AWS
*   **ECS** (Elastic Container Service) runs containers from those images. **Fargate** runs them for you, on servers you never create, patch, or log in to
*   The **Network Load Balancer** gives your API one address that never changes. Each Fargate task gets a new IP address when it starts, so the load balancer finds it for you


## Do the pages in order

Each page uses something the page before it created. Do not skip steps.

0.  [AWS - Create an Elastic Container Registry (ECR) Repository](https://ensign.instructure.com/courses/27288/modules/items/3250493)
0.  [AWS - Copy the API Container Image to the ECR Repo](https://ensign.instructure.com/courses/27288/modules/items/3250494)
0.  [AWS - Create a Network Load Balancer (NLB)](https://ensign.instructure.com/courses/27288/pages/aws-create-a-network-load-balancer-nlb)
0.  [AWS - Running the Docker Image in Elastic Container Service (ECS) Fargate](https://ensign.instructure.com/courses/27288/pages/aws-running-the-docker-image-in-elastic-container-service-ecs-fargate)


Use these names exactly. A later page asks you to pick each one from a list:

| Page | You create | Name |
| ---- | ---------- | ---- |
| ECR | Repository | `cs415-ecr-api` |
| NLB | Load balancer | `cs415-api-nlb` |
| NLB | Security group | `cs415-api-web-access-security-group` |
| NLB | Target group | `cs415-target-group` |
| ECS | Task definition | `cs415-api-task` |
| ECS | Container | `cs415-api` |
| ECS | Cluster | `cs415-ecs-cluster-api` |
| ECS | Service | `cs415-api-fargate-service` |


*   Ponder: [Ponder and Respond #5 - Building Faith and Infrastructure](https://ensign.instructure.com/courses/27288/assignments/1629925)
*   Prove: [Show API Running in ECS](https://ensign.instructure.com/courses/27288/modules/items/3250498)

Both are due Saturday, November 28.

*Check:* Why does your image have to be in ECR before you can create the ECS task definition?


# Push Your Image to ECR

Pushing your image is the only step this week that uses the command line, and every later step depends on it. Let's do it together, so you leave today with your image in AWS.

Follow [AWS - Create an Elastic Container Registry (ECR) Repository](https://ensign.instructure.com/courses/27288/modules/items/3250493) and [AWS - Copy the API Container Image to the ECR Repo](https://ensign.instructure.com/courses/27288/modules/items/3250494).

## Before you build

Your code is copied into the image when you build it. Check these first, so a mistake does not ride along to AWS:

*   In `settings.py`, `HOST` is your `cs415_db_ip`, not `localhost`
*   In `settings.py`, `ALLOWED_HOSTS = ['*']`. Without it, Django answers `400 Bad Request` to any address but `localhost`
*   The first line of your `Dockerfile` is `FROM debian:trixie-slim`. Fargate runs Intel/AMD processors, and an image built for an Apple-silicon Mac will not start there


## Create the repository

0.  In the AWS Management Console, make sure the region is **us-west-2 (Oregon)**
0.  Open **Elastic Container Registry** and click **Create**
0.  Name the repository `cs415-ecr-api`, keep the defaults, and click **Create**
0.  Copy the repository's **URI**. It looks like `123456789012.dkr.ecr.us-west-2.amazonaws.com/cs415-ecr-api`


## Build, tag, log in, push

Run these in your `cs415_api` folder, where your `Dockerfile` and `docker-compose.yml` are.

0.  Build the image. On an Apple-silicon Mac this can take several minutes:
    ```bash
    docker buildx bake --provenance=false
    ```
0.  Give the image a second name, your repository's URI:
    ```bash
    docker tag image_cs415_api <ECR-repo-URI>
    ```
    Check that it appears in Docker Desktop under **Images**
0.  Make sure the AWS CLI can reach your lab:
    ```bash
    aws sts get-caller-identity
    ```
0.  Log Docker in to your registry. The **base URI** is your repository URI without `/cs415-ecr-api` on the end. Both places say `us-west-2`:
    ```bash
    aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin <ECR-base-URI>
    ```
    Wait for `Login Succeeded`
0.  Push the image. Let it run through the break:
    ```bash
    docker push <ECR-repo-URI>
    ```
0.  In the console, open your repository. You should see one image tagged `latest`


`--provenance=false` keeps the build from attaching an extra record to your image, so your repository holds one plain image.

*Check:* Why do you tag the image with your repository's URI before you push it?


# Break

Take 10 minutes. Leave your `docker push` running.


# Work Time

Use the rest of class on your assignments while I help. Work in this order:

0.  Finish your push, and see the `latest` image in your ECR repository
0.  [AWS - Create a Network Load Balancer (NLB)](https://ensign.instructure.com/courses/27288/pages/aws-create-a-network-load-balancer-nlb). It takes a few minutes to become active
0.  [AWS - Running the Docker Image in Elastic Container Service (ECS) Fargate](https://ensign.instructure.com/courses/27288/pages/aws-running-the-docker-image-in-elastic-container-service-ecs-fargate). Wait until your service shows **1/1 Tasks running**
0.  Take the three screenshots for [Show API Running in ECS](https://ensign.instructure.com/courses/27288/modules/items/3250498)
0.  Finish your [Ponder and Respond #5 - Building Faith and Infrastructure](https://ensign.instructure.com/courses/27288/assignments/1629925)


When you test your load balancer's DNS name, type `http://` in front of it. It does not answer `https://`.

If you finish early, help a classmate who is stuck.


# Wrap Up & Parting Thoughts

## Due Saturday, November 28

0.  [Show API Running in ECS](https://ensign.instructure.com/courses/27288/modules/items/3250498): 3 screenshots, 20 points. Your lab must be running when you take them:
    *   Your load balancer's properties page, showing its **DNS name**
    *   A browser at your DNS name's `/users/` endpoint, with data showing
    *   A browser at your DNS name's `/addresses/` endpoint, with data showing
0.  [Ponder and Respond #5 - Building Faith and Infrastructure](https://ensign.instructure.com/courses/27288/assignments/1629925): read Elder Scott's talk and Proverbs 3:5-6, then answer all four questions with specific examples from your own work. The Ponder and Respond assignments together are about a quarter of your course grade


## Working on your own this week

*   Start your lab before every work session. Your database runs only while your lab session is active
*   If the `aws` command says your token has expired, get new credentials from **AWS Details** in your lab
*   Keep everything you built: the database instance, the repository, the load balancer, and the service. Never **terminate** `cs415_postgres`
*   If you get stuck, email me. Do not wait until Saturday


## What is next

No class Wednesday. Happy Thanksgiving! We meet again Monday, November 30. Watch Canvas for updates.


## Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module5/Lec09-Mon_Nov_23
0.  Pick **one** to answer in your Canvas reply:
    1.  In one sentence each, what do ECR, the Network Load Balancer, and ECS Fargate do in your project?
    1.  How far did you get today: repository created, image pushed, load balancer created, service running?
    1.  What is one thing about this week's AWS steps that you are still unsure about?


