CS415 - Wednesday, November 04 - Lecture 4 - Module 2

# Topics:
* [Today's Spiritual Thought](#todays-spiritual-thought)
* [Check-In and Q&A](#check-in-and-qa)
* [How a Request Flows Through Django REST Framework](#how-a-request-flows-through-django-rest-framework)
* [Django Setup and Connecting to the Database](#django-setup-and-connecting-to-the-database)
* [Break](#break)
* [Your First GET Endpoint: users](#your-first-get-endpoint-users)
* [Work Time](#work-time)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Action Items

0.  Prayer
0.  Spiritual Thought
0.  Check-In and Q&A
0.  How a Request Flows Through Django REST Framework
0.  Django Setup and Connecting to the Database
0.  Break
0.  Your First GET Endpoint: users/
0.  Work Time
0.  Exit Ticket
0.  Wrap Up & Parting Thoughts


# Today's Spiritual Thought

Since we last met, what is one small step you took that worked? The Lord teaches line upon line, and this course is built the same way: each week adds to the last. Today the line is one API endpoint, and by the end of class you will have seven.


## Scripture

> For he will give unto the faithful line upon line, precept upon precept; and I will try you and prove you herewith.
>
> -- [Doctrine and Covenants 98:12](https://www.churchofjesuschrist.org/study/scriptures/dc-testament/dc/98?lang=eng&id=p12#p12)


## Discussion Questions

0.  Where did you feel stuck this week, and what helped you get unstuck?
0.  How can you help a classmate with a small step this hour?


# Check-In and Q&A

Today's work depends on your database from the last meeting. Let's make sure it is there before we build on it.

## Is your database ready?

Check these four things on your own computer:

0.  Docker Desktop shows your database container with status Running and port `5432:5432`
0.  `docker exec -it container_cs415_db bash` opens a shell in the container
0.  `psql -U postgres` and then `\c cs415` connect you to your database
0.  `\dt` lists 7 tables, and `select * from webuser;` shows rows

If any of these fail, tell me now. If yours works, you may be paired with someone whose does not.

## Is Python ready?

Type `python --version` in a terminal (or `python3 --version`). You need version 3.9, 3.10, or 3.11. Is your `cs415_api` repository cloned and open in VS Code?

For help with your database, see the [PostgreSQL Database](https://ensign.instructure.com/courses/27288/pages/postgresql-database) and [Accessing the Data](https://ensign.instructure.com/courses/27288/pages/accessing-the-data) pages. If Docker shows an error, read its logs first.

*Check:* What command shows how many tables your database has?


# How a Request Flows Through Django REST Framework

**Django REST Framework** (DRF) is the toolkit you will use to build your API. Before you write code, learn the path a request travels. Study these three pages from the Preparation & Resources:

0.  [DRF Architecture data flow](https://ensign.instructure.com/courses/27288/modules/items/3250460)
0.  [DRF Sequence data flow](https://ensign.instructure.com/courses/27288/modules/items/3250461)
0.  [API Creation Flow](https://ensign.instructure.com/courses/27288/modules/items/3250462)

## One request, four files

When a browser asks for `/users/`, it passes through four files in your project:

0.  `urls.py` matches the URL pattern and picks the view
0.  `views.py` holds the view class that decides how to respond to GET, POST, and other request types
0.  `serializers.py` holds the **serializer**, which decides which fields to show and turns rows into JSON
0.  `models.py` holds the **model**, which is the Django version of one database table

The database sits behind the model. The answer travels back the same way, ending as an HTTP response with JSON.

## How to add an endpoint

Work backward from the data:

0.  Model: make sure it matches the table
0.  Serializer: create a serializer that uses the model
0.  View: create a view that uses the serializer
0.  URL: add a URL pattern that points to the view

The database in these diagrams is drawn as MySQL in some places. Ours is PostgreSQL, and the flow is the same.

*Check:* Which file decides that the password column is never shown?


# Django Setup and Connecting to the Database

Follow the [Django - Setup and Run](https://ensign.instructure.com/courses/27288/modules/items/3250464) page in order. Django runs on Python, inside a **virtual environment**, a private folder of packages for one project.

## Demo and your turn

Work in the `cs415_api` project in VS Code. Your database container must be running.

0.  Check Python: `python --version` (or `python3 --version`)
0.  Create the virtual environment: `python -m venv env`
0.  Activate it. Windows: `env\Scripts\activate`. Mac: `source env/bin/activate`. You should see `(env)` at the start of your prompt
0.  Install the packages listed on the page with `pip install`, one at a time. If any fails, ask for help
0.  Save the list with `pip freeze > requirements.txt`
0.  Create the project: `django-admin startproject cs415`
0.  Move into the project folder: `cd cs415`
0.  Edit `cs415/settings.py` as shown on the page: allowed hosts, CORS settings, installed apps, middleware, and the `DATABASES` section that points at your container
0.  Start the server: `python manage.py runserver`, then open `http://127.0.0.1:8000/` and look for the Django welcome page
0.  Press Ctrl+C to stop the server
0.  Have Django read your tables: `python manage.py inspectdb > ./cs415/models.py`
0.  Open `models.py` and check that it has a class for each of your tables

Always have `(env)` in your prompt when you work on this project. Run `manage.py` commands from the folder that contains `manage.py`.

*Check:* How do you know your virtual environment is active?


# Break

Take 10 minutes.


# Your First GET Endpoint: users/

You will build one **GET endpoint** for the `WebUser` table, then repeat the pattern for the rest. Follow [DRF Model, Serializer, View - Creating a GET Endpoint](https://ensign.instructure.com/courses/27288/modules/items/3250465) and [DRF URLS - Accessing the Endpoints](https://ensign.instructure.com/courses/27288/modules/items/3250466).

## Demo and your turn

Work in the folder that contains `manage.py`, with `(env)` in your prompt.

0.  Model: your `models.py` from `inspectdb` already has the table
0.  Serializer: create `serializers.py` with a serializer for the model. Choose the fields to return. Do not return the `password` field
0.  View: create `views.py` with a view class that has a `get` function returning all the rows
0.  URL: in `urls.py`, import your view and add the URL pattern `users/`
0.  Start the server with `python manage.py runserver`
0.  Open `http://127.0.0.1:8000/users/` in your browser. You should see the data from your database as JSON

You can also test with **Postman**, a free tool for sending requests to an API. Choose GET, paste the URL, and click Send.

## Repeat for every table

Each table gets its own serializer, view, and URL. Use these URL names:

*   `users/`
*   `address-types/`
*   `addresses/`
*   `phone-types/`
*   `phones/`
*   `user-infos/`
*   `pages/`

*Check:* Which of the four files do you change to add a new endpoint, and in what order?


# Work Time

Use the rest of class on your assignments while I help:

0.  Finish your [Pulling API Data](https://ensign.instructure.com/courses/27288/modules/items/3250468) endpoints and screenshots
0.  Finish your [Ponder and Respond #2 - Connecting Principles](https://ensign.instructure.com/courses/27288/assignments/1629922)


Push your work to GitHub as you go. If you finish early, help a classmate who is stuck.


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module2/Lec04-Wed_Nov_04
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement

## Exit Ticket Questions

Pick **one** to answer in your Canvas reply:

0.  Name the four files you change to add a GET endpoint, in the order you change them, and say what each one does.
0.  What does your `users/` endpoint return so far? Does it include the password field, and why or why not?
0.  What is one thing about Django or the API that you are still unsure about?


# Wrap Up & Parting Thoughts

## Due this week

0.  [Pulling API Data](https://ensign.instructure.com/courses/27288/modules/items/3250468): upload seven screenshots, one for each of `users/`, `address-types/`, `addresses/`, `phone-types/`, `phones/`, `user-infos/`, and `pages/`. Each must show the URL and all of the JSON
0.  [Ponder and Respond #2 - Connecting Principles](https://ensign.instructure.com/courses/27288/assignments/1629922): read the assigned talk and scripture, then answer all four reflection questions thoughtfully

The Ponder and Respond assignments together are about a quarter of your course grade. Watch Canvas for updates and due dates.

## Keep your work

Push your `cs415_db` and `cs415_api` repositories to GitHub. Next week builds directly on both.

## What is next

Next meeting you will add documentation to your API with Swagger, and build a filtered GET endpoint plus POST and PATCH endpoints.


