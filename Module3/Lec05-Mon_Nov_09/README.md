CS415 - Monday, November 09 - Lecture 5 - Module 3

# Topics:
* [Opening Exercises](#opening-exercises)
* [Week 3 Overview and Check-In](#week-3-overview-and-check-in)
* [HTTP Methods: Designing a Website's Endpoints](#http-methods-designing-a-websites-endpoints)
* [Swagger: Documenting Your API](#swagger-documenting-your-api)
* [Filtered `GET` Endpoints](#filtered-get-endpoints)
* [Break](#break)
* [Work Time](#work-time)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Announcements

0.  For [API Documentation using Swagger](https://ensign.instructure.com/courses/27288/assignments/1629915), your API may run locally with `python manage.py runserver`. It does not need to run in Docker until Week 4. Your database still runs in its Docker container.


# Opening Exercises

### Song *Hymn: #140 [Did You Think to Pray?](https://www.churchofjesuschrist.org/media/music/songs/did-you-think-to-pray)*

### Prayer

### Today's Spiritual Thought

Every endpoint you build this week answers a request. A client asks, and it trusts the server to answer the way its documentation promises. The Lord invites us to ask, seek, and knock, and He promises to answer, though not always on our schedule or in the shape we expect. What keeps you from asking, whether in prayer or in class?


### Scripture

> Ask, and it shall be given you; seek, and ye shall find; knock, and it shall be opened unto you:
>
> For every one that asketh receiveth; and he that seeketh findeth; and to him that knocketh it shall be opened.
>
> -- [Matthew 7:7-8](https://www.churchofjesuschrist.org/study/scriptures/nt/matt/7?lang=eng&id=p7-p8#p7)


### Quote

> Hope is not knowledge, but rather the abiding trust that the Lord will fulfill His promise to us. It is confidence that if we live according to God’s laws and the words of His prophets now, we will receive desired blessings in the future. It is believing and expecting that our prayers will be answered. It is manifest in confidence, optimism, enthusiasm, and patient perseverance.
>
> -- President Dieter F. Uchtdorf, ["The Infinite Power of Hope"](https://www.churchofjesuschrist.org/study/general-conference/2008/10/the-infinite-power-of-hope?lang=eng)


### Discussion Questions

0.  What makes it hard to ask for help, in class or in prayer?
0.  When has an answer come to you in a different form than you expected?
0.  API documentation tells a client what it may ask for and what it will get back. What has the Lord told us about how to ask Him?

You will read this scripture and talk again for [Ponder and Respond #3 - Faith in Coding](https://ensign.instructure.com/courses/27288/assignments/1629923).


# Week 3 Overview and Check-In

This course is **incremental**. Each week builds on the last, and nothing you build is thrown away. This week your API learns to write to the database and to explain itself.

## Where you are

In Week 2 you ran PostgreSQL in a container and built seven `GET` endpoints that return whole tables as JSON. This week adds to that same `cs415_api` project.

## This week

0.  Design the endpoints a website needs, and add Swagger documentation to your API
0.  Build `GET` endpoints that return one record instead of a whole table
0.  Add `POST` endpoints that create records and `PATCH` endpoints that update them

Each week has three parts on Canvas: **Preparation & Resources** (read first), **Ponder**, and **Prove**. Read these in order as you work:

0.  [Swagger - Testing and Documenting the Endpoints](https://ensign.instructure.com/courses/27288/pages/swagger-testing-and-documenting-the-endpoints)
0.  [Django - Filtered `GET` Endpoints](https://ensign.instructure.com/courses/27288/pages/django-filtered-get-endpoints)
0.  [Django - `POST` Endpoints](https://ensign.instructure.com/courses/27288/pages/django-post-endpoints)
0.  [Django - `PATCH` Endpoints](https://ensign.instructure.com/courses/27288/pages/django-patch-endpoints)


*   Ponder: [Ponder and Respond #3 - Faith in Coding](https://ensign.instructure.com/courses/27288/assignments/1629923)
*   Prove: [API Documentation using Swagger](https://ensign.instructure.com/courses/27288/assignments/1629915)

Both are due Saturday, November 14.

## Check-in

Do all seven of your Week 2 `GET` endpoints work? If not, finish them first during work time, because everything this week is built on them. Ask for help early. You are the agent of your own learning, and class time is for working while I help.

*Check:* Which file from Week 2 will you change most this week, and why?


# HTTP Methods: Designing a Website's Endpoints

Every HTTP request names a **method** that says what the client wants done.

| Method | What it does | In our project |
| ------ | ------------ | -------------- |
| `GET`    | Reads data | Built in Week 2 |
| `POST`   | Creates a new record | This week |
| `PATCH`  | Changes some fields of an existing record | This week |
| `PUT`    | Replaces a whole record | Not built |
| `DELETE` | Removes a record | Not built |


## Design exercise

Picture the website our database supports. For each feature, which method and which URL would you use? We will fill these in on the board together.

*   Sign up for an account
*   View my profile
*   Change my email address
*   Add a phone number
*   Close my account
*   Log in


## Why not let the website talk to the database directly?

The API stands between users and the database. Because it does, it can:

*   Keep columns like `password` from ever being sent out
*   Reject bad input before it reaches a table
*   Let the database change without breaking every website and app that uses it
*   Keep the database login on the server, where users never see it

*Check:* Why is logging in usually a `POST` and not a `GET`, even though it does not add a row?


# Swagger: Documenting Your API

The people who build a website's front end are often not the people who built its API. They need to know which URLs to call, what data to send, and what data comes back. Without documentation, they have to guess or read your code.

**Swagger** is a set of tools built on the **OpenAPI** standard. It reads your code, builds a web page that documents your API, and lets you test each endpoint from the browser. You installed the `drf-yasg` package that provides it in Week 2.

Follow  [Swagger - Testing and Documenting the Endpoints](https://ensign.instructure.com/courses/27288/pages/swagger-testing-and-documenting-the-endpoints).


## Demo and your turn

Work in the folder that contains `manage.py`, with `(env)` in your prompt.

0.  In `urls.py`, add `re_path` to the `django.urls` import, and add these two imports:
    ```
    from drf_yasg.views import get_schema_view
    from drf_yasg import openapi
    ```
0.  Below the imports, add the `schema_view = get_schema_view(...)` block from the page
0.  Add this pattern at the top of `urlpatterns`:
    ```
    re_path(r'^swagger/$', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    ```
0.  In `views.py`, add `from drf_yasg.utils import swagger_auto_schema` and `from drf_yasg import openapi`
0.  Start the server: `python manage.py runserver`
0.  Open `http://127.0.0.1:8000/swagger/`. Expand `GET /users/`, click **Try it out**, then **Execute**
0.  Notice that the response has no description. Add this line directly above `def get(self, request):` in `WebUserAPIView`:
    ```
    @swagger_auto_schema(responses={200: WebUserSerializer(many=True)})
    ```
0.  Reload `/swagger/` and look at the response description again


## Repeat for every `GET`

Add the decorator to all seven of your `GET` methods. Each one names its own view's serializer.

*Check:* Where does Swagger get the list of fields it shows for a response?


# Filtered `GET` Endpoints

Your Week 2 endpoints return every row in a table. Most of the time, a client wants one record: one user's profile, or one address. A **filtered `GET` endpoint** takes an ID from the URL and returns only that record.

Follow [Django - Filtered `GET` Endpoints [Django - Filtered `GET` Endpoints](https://ensign.instructure.com/courses/27288/pages/django-filtered-get-endpoints).

## Demo and your turn

0.  In `views.py`, create a new class `SingleWebUserAPIView` that inherits from `APIView`
0.  Give it a `get` method that takes `self`, `request`, and `web_user_id`
0.  Inside it, fetch one row with `Webuser.objects.get(web_user_id=web_user_id)`, pass it to your serializer (one object, so no `many=True`), and return `Response(serializer.data)`
0.  In `urls.py`, import the new view and add the pattern `users/user/<int:web_user_id>` that points to it
0.  Open `http://127.0.0.1:8000/users/user/1`. You should see exactly one user
0.  Find the new endpoint on your `/swagger/` page and try it there

The `<int:web_user_id>` part of the pattern takes the number from the URL and passes it to `get` as `web_user_id`.


## When it breaks

Open `http://127.0.0.1:8000/users/user/999`, an ID that does not exist. Read the error page from the top. What is the exception's name, and which line of your `views.py` raised it?

A client should get **404 Not Found** for a missing record, not a server error. Fixing that is optional this week. If you want to try, look up Django's `get_object_or_404`.


## Repeat for every table

Add a filtered view and URL for `Addresstype`, `Useraddress`, `Phonetype`, `Userphone`, `Userinfo`, and `Pagedata`. Check that each one appears on your `/swagger/` page.

*Check:* You request `users/user/999` and get an error page. What does the error tell you, and what should the API return instead?


# Break

Take 10 minutes.


# Work Time

Use the rest of class on your assignments while I help:

0.  If any of your Week 2 `GET` endpoints are missing, finish them first
0.  Add Swagger, following [Swagger - Testing and Documenting the Endpoints](https://ensign.instructure.com/courses/27288/modules/items/3250473)
0.  Build a filtered `GET` endpoint for all seven tables, following [Django - Filtered `GET` Endpoints](https://ensign.instructure.com/courses/27288/modules/items/3250474)
0.  Start your [Ponder and Respond #3 - Faith in Coding](https://ensign.instructure.com/courses/27288/modules/items/3250479)


Push your work to GitHub as you go. If you finish early, help a classmate who is stuck.


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module3/Lec05-Mon_Nov_09
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement


## Exit Ticket Questions

Pick **one** to answer in your Canvas reply:

0.  Pick one feature of a website, such as signing up or changing your email. Which HTTP method and URL would you use, and why that method?
0.  What does your `/swagger/` page show so far? How many endpoints, and which ones show a response format?
0.  What is one thing about Swagger or filtered `GET` endpoints that you are still unsure about?


# Wrap Up & Parting Thoughts

## Before the next meeting

`POST` and `PATCH` endpoints build on today's work, so have these working before we meet again:

0.  `/swagger/` loads, and every `GET` on it shows a response format
0.  A filtered `GET` endpoint works for all seven tables


## Also this week

0.  Read the talk and scripture for [Ponder and Respond #3 - Faith in Coding](https://ensign.instructure.com/courses/27288/modules/items/3250479), then answer all four questions with specific examples from your own work. The Ponder and Respond assignments together are about a quarter of your course grade
0.  Read [API Documentation using Swagger](https://ensign.instructure.com/courses/27288/modules/items/3250478) so you know which screenshots to capture
0.  Watch Canvas for updates, and check due dates there


## What is next

Next meeting you will add `POST` endpoints that create records and `PATCH` endpoints that update them.


