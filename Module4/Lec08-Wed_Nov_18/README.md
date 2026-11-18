CS415 - Wednesday, November 18 - Lecture 8 - Module 4

# Topics:
* [Opening Exercises](#opening-exercises)
* [Check-In: Is Your Database in AWS](#check-in-is-your-database-in-aws)
* [Point Django at AWS](#point-django-at-aws)
* [The API in a Container](#the-api-in-a-container)
* [Trace a Request, Read the Logs](#trace-a-request-read-the-logs)
* [Mid-Term Screenshot Checklist](#mid-term-screenshot-checklist)
* [Work Time](#work-time)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Announcements

0.  I need to leave early today. You are free to use the classroom right up until 12:40.
0.  The [Mid-Term Assignment](https://ensign.instructure.com/courses/27288/modules/items/3250488) and [Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489) are due this Saturday, November 21.
0.  Start your AWS Academy lab now. Your database runs only while your lab session is active.
0.  The first line of your `Dockerfile` must be `FROM --platform=linux/amd64 python:3.13-slim`. If your copy of [Running the API in a Container](https://ensign.instructure.com/courses/27288/modules/items/3250486) says `python:3.11-slim`, use `3.13` instead. Django 6 needs Python 3.12 or newer.


# Opening Exercises

### Song *Hymn: #129 [Where Can I Turn for Peace?](https://www.churchofjesuschrist.org/media/music/songs/where-can-i-turn-for-peace)*

### Prayer

### Today's Spiritual Thought

Since we last met, is your database answering from AWS, and what is still broken? A container rarely works the first time, and fixing it starts with looking honestly at what is wrong. What keeps you from asking for the help you need?


### Scripture

> If any of you lack wisdom, let him ask of God, that giveth to all men liberally, and upbraideth not; and it shall be given him.
>
> -- [James 1:5](https://www.churchofjesuschrist.org/study/scriptures/nt/james/1?lang=eng&id=p5#p5)



### Discussion Questions

0.  When your code breaks, what do you do first? What would that same habit look like in your spiritual life?
0.  James says God gives wisdom "liberally, and upbraideth not." How does knowing you will not be scolded change how willing you are to ask, of God or of a classmate?
0.  Who could the app you are building help, and how?


# Check-In: Is Your Database in AWS?

Your API connects to your database in AWS next. Check the database before you build on it.

From now on, start your AWS Academy lab before you do any work for this class. Your database runs only while your lab session is active.

## Is your database ready?

0.  In EC2, your `cs415_postgres` instance is **Running**, and its public IPv4 address is your `cs415_db_ip`
0.  pgAdmin connects to `AWS - cs415`
0.  The `webuser` table has rows


If any of these fail, tell me now. If yours works, you may be paired with someone whose does not.

For help, see [Elastic IP Addresses](https://ensign.instructure.com/courses/27288/modules/items/3250483) and [Deploying the Database to AWS](https://ensign.instructure.com/courses/27288/modules/items/3250484).

*Check:* You started a new lab session today. Which address do you connect to, and why did it not change?


# Point Django at AWS

Until now, your API has read from the database container on your laptop. One setting moves it to AWS.

Follow [Connect the API to a Remote Database](https://ensign.instructure.com/courses/27288/modules/items/3250485).

## Demo and your turn

0.  In EC2, copy the **Public IPv4 address** of `cs415_postgres`. It is your `cs415_db_ip`
0.  Open `settings.py` in your `cs415_api` project
0.  In the `DATABASES` section, paste the address as the `HOST` value, in quotes. Leave the other values as they are
0.  Save the file
0.  With `(env)` in your prompt, run `python manage.py runserver` in the folder that contains `manage.py`
0.  Open `http://127.0.0.1:8000/users/`


## Prove it

How do you know the data came from AWS and not from your laptop?

0.  Stop your old database container: `docker stop container_cs415_db`
0.  Reload `/users/`. It should still work
0.  In `/swagger/`, PATCH one user's `last_name`
0.  In pgAdmin, look at that row in `webuser` on `AWS - cs415`


Your `DATABASES` section is Mid-Term screenshot 4.

*Check:* Why will your `HOST` value keep working after your lab session stops and starts again?


# The API in a Container

`python manage.py runserver` is a development server. It is fine for testing on your own laptop, but it is not built for real traffic. Now your API moves into a container that runs **Apache**, a production web server.

Inside the container:

*   Debian Linux and Python
*   Apache, listening on port 80
*   **mod_wsgi**, which lets Apache run your Django code
*   Your project in `/django/`, with its own virtual environment in `/django/env`


Follow [Running the API in a Container](https://ensign.instructure.com/courses/27288/modules/items/3250486). For a refresher on Docker, see [Running a Container](https://ensign.instructure.com/courses/27288/pages/running-a-container) and [Docker Compose](https://ensign.instructure.com/courses/27288/pages/docker-compose).

## Demo and your turn

0.  In `settings.py`, add these two lines right below `STATIC_URL = 'static/'`:
    ```
    STATIC_ROOT = '/django/site/public/static'
    MEDIA_ROOT = '/django/site/public/media'
    ```
    Swagger's styles and scripts are **static files**. The build copies them to `STATIC_ROOT`, and Apache serves them from there
0.  In your `cs415_api` folder, create `000-default.conf` with the Apache settings from the page. It tells Apache to run `/django/cs415/wsgi.py` with the Python in `/django/env`
0.  In the same folder, create the `Dockerfile` from the page, but make its first line:
    ```
    FROM --platform=linux/amd64 python:3.13-slim
    ```
    Django 6 needs Python 3.12 or newer, and the mod_wsgi that Apache installs in this image uses Python 3.13
0.  Make sure `requirements.txt` sits in the same folder as `manage.py`. The `Dockerfile` copies only the `cs415` folder and installs from there. Keep your `env` folder out of `cs415`, so it is not copied into the image
0.  Create `docker-compose.yml` in the `cs415_api` folder. Type it from the page's screenshot, and watch the indentation. It names the service `cs415-api`, the image `image_cs415_api`, and the container `container_cs415_api`, and maps port `80:80`
0.  In the folder with `docker-compose.yml`, run `docker-compose up -d`. The first build can be slow, so start it before the break
0.  In Docker Desktop, check that `container_cs415_api` is **Running** with port `80:80`
0.  Open `http://localhost/swagger` and `http://localhost/users`


*Check:* The `Dockerfile` copies `./cs415/` into `/django/`. Which paths in `000-default.conf` depend on that line?


# Trace a Request, Read the Logs

Your app now spans three places: a container, your laptop, and a server in AWS. When something breaks, you need to know which hop failed.

## Trace it aloud

Pair up. One partner traces a request to `http://localhost/users` out loud, hop by hop:

0.  The browser sends the request to port 80 on your laptop
0.  Docker forwards it to port 80 in the container
0.  Apache hands it to mod_wsgi, which runs Django
0.  Django matches the URL and runs your view
0.  The view asks PostgreSQL for data, across the internet
0.  The AWS security group lets port 5432 through
0.  PostgreSQL checks `pg_hba.conf` and your password, then answers


The other partner names one thing that could break at each hop. Then switch, and trace `http://localhost/swagger`.

## Where to look

*   **Is the container running?** Look in Docker Desktop, or run `docker ps -a`. A container that keeps restarting has a start-up problem
*   **Apache's start-up messages:** `docker logs container_cs415_api`
*   **Python errors from your code:** `docker exec -it container_cs415_api tail -n 30 /django/site/logs/error.log`
*   **Changed your code?** Rebuild with `docker-compose up -d --build`. Without `--build`, the old image keeps running and your change seems to do nothing


*Check:* `/swagger` loads, but `/users` returns a 500 error. Where do you look, and what is a likely cause?


# Mid-Term Screenshot Checklist

The [Mid-Term Assignment](https://ensign.instructure.com/courses/27288/modules/items/3250488) is worth 50 points and is due Saturday, November 21. Upload these 7 screenshots. The assignment shows an example of each.

0.  The AWS Management Console showing your `cs415_postgres` instance in a **Running** state
0.  pgAdmin's connection properties for `AWS - cs415`, showing your instance's IP address. Right-click the server, choose **Properties**, then the **Connection** tab
0.  pgAdmin showing all the data in the `webuser` table, connected to AWS
0.  Your `settings.py` showing the `DATABASES` section
0.  Docker Desktop showing your API container in a **Running** state
0.  A browser at `http://localhost/users`, with its data showing
0.  A browser at `http://localhost/swagger`


*Check:* Screenshot 6 shows data at `http://localhost/users`. What must be running when you take it?


# Work Time

Use the rest of class on your assignments while I help:

0.  Get `container_cs415_api` running, following [Running the API in a Container](https://ensign.instructure.com/courses/27288/modules/items/3250486)
0.  Take the rest of your [Mid-Term Assignment](https://ensign.instructure.com/courses/27288/modules/items/3250488) screenshots and submit them
0.  Finish your [Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489)


Push your work to GitHub as you go. If you finish early, help a classmate who is stuck.


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module4/Lec08-Wed_Nov_18
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement


## Exit Ticket Questions

Pick **one** to answer in your Canvas reply:

0.  Trace a request to `http://localhost/users` from your browser to your database. Name one place it could fail, and where you would look.
0.  Which Mid-Term screenshots do you have so far, and which are left?
0.  What is one thing about your container (the `Dockerfile`, `docker-compose.yml`, or Apache) that you are still unsure about?


# Wrap Up & Parting Thoughts

## Due Saturday, November 21

0.  [Mid-Term Assignment](https://ensign.instructure.com/courses/27288/modules/items/3250488): 7 screenshots, 50 points. Your lab must be running for the screenshots that show your data
0.  [Ponder and Respond #4 - Healing Through Code](https://ensign.instructure.com/courses/27288/modules/items/3250489): answer all four questions thoughtfully, with specific examples from your own work. The Ponder and Respond assignments together are about a quarter of your course grade


## Keep your work

Push `cs415_db` and `cs415_api` to GitHub, including your `Dockerfile`, `docker-compose.yml`, and `000-default.conf`. Next week builds on them.

Keep your `cs415_postgres` instance. Stopping it is fine; terminating it deletes your database.


## What is next

Next week your API leaves your laptop too. Watch Canvas for updates.


