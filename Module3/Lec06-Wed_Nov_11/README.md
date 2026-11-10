CS415 - Wednesday, November 11 - Lecture 6 - Module 3

# Topics:
* [Opening Exercises](#opening-exercises)
* [HTTP Methods: Designing a Website's Endpoints](#http-methods-designing-a-websites-endpoints)
* [Check-In and Q&A](#check-in-and-qa)
* [POST Endpoints: Creating Records](#post-endpoints-creating-records)
* [PATCH Endpoints: Updating Records](#patch-endpoints-updating-records)
* [Work Time](#work-time)
* [Submitting Today's Exit Ticket](#submitting-todays-exit-ticket)
* [Wrap Up & Parting Thoughts](#wrap-up--parting-thoughts)


------------------------------------------------------------
# Announcements

0.  Today's class will end early at 11:55 AM
    *   You may still submit your exit ticket until 1 PM
0.  [API Documentation using Swagger](https://ensign.instructure.com/courses/27288/modules/items/3250478) and [Ponder and Respond #3 - Faith in Coding](https://ensign.instructure.com/courses/27288/modules/items/3250479) are due Saturday, November 14.


# Opening Exercises

### Song *Hymn: #3 [Now Let Us Rejoice](https://www.churchofjesuschrist.org/media/music/songs/now-let-us-rejoice)*

### Prayer

### Today's Spiritual Thought

What gives you hope?

Today we will learn about more HTTP methods besides `GET`. A `PATCH` request changes only the fields that need changing, and the record keeps its ID. Repentance through the Atonement of Jesus Christ is like that: we change what needs to change, and we remain who we are, children of God. What gives you the hope to keep making changes, one step at a time?


### Scripture

> And what is it that ye shall hope for? Behold I say unto you that ye shall have hope through the atonement of Christ and the power of his resurrection, to be raised unto life eternal, and this because of your faith in him according to the promise.
>
> Wherefore, if a man have faith he must needs have hope; for without faith there cannot be any hope.
>
> -- [Moroni 7:41-42](https://www.churchofjesuschrist.org/study/scriptures/bofm/moro/7?lang=eng&id=p41-p42#p41)

### Quote

> Faith, hope, and charity complement each other, and as one increases, the others grow as well. Hope comes of faith, for without faith, there is no hope. In like manner faith comes of hope, for faith is “the substance of things hoped for.”
>
> -- President Dieter F. Uchtdorf, ["The Infinite Power of Hope"](https://www.churchofjesuschrist.org/study/general-conference/2008/10/the-infinite-power-of-hope?lang=eng)


### Discussion Questions

0.  If the Lord wants us to be hopeful, why, then, does he allow us to feel despair?
0.  How is fixing a bug one step at a time like the way the Lord asks us to change?
0.  What helps you keep hoping when your code, or your life, does not work the first time?


# HTTP Methods: Designing a Website's Endpoints

Every HTTP request names a **method** that says what the client wants done.  These methods correspond to actions the server can make.  In the context of a webapp that is connected to a database, these HTTP methods correspond to SQL commands.

| Method   | SQL Command | What it does                        | In our project |
| -------- | ----------- | ----------------------------------- | -------------- |
| `GET`    | `SELECT`    | Read data                           | Built in Week 2 |
| `POST`   | `INSERT`    | Create a new record                 | *Built this week* |
| `PUT`    | `UPDATE`    | Entirely replace an existing record | ~~Not part of this app~~ |
| `PATCH`  | `UPDATE`    | Change parts of an existing record  | *Built this week* |
| `DELETE` | `DELETE`    | Remove a record                     | ~~Not part of this app~~ |


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


# Check-In and Q&A

`POST` and `PATCH` endpoints build on your Swagger page and your filtered `GET` endpoints. Check them before you add to them.

## Is your API ready?

Start your database container and your server (`python manage.py runserver`), then check these three things:

0.  `http://127.0.0.1:8000/swagger/` loads and lists your endpoints
0.  Each `GET` on your Swagger page shows a response format
0.  `http://127.0.0.1:8000/users/user/1` returns exactly one user

If any of these fail, tell me ASAP. If your endpoints work, help someone whose do not.

For help, see [Swagger - Testing and Documenting the Endpoints](https://ensign.instructure.com/courses/27288/modules/items/3250473) and [Django - Filtered GET Endpoints](https://ensign.instructure.com/courses/27288/modules/items/3250474).

*Check:* What is the difference between the URL that returns all users and the URL that returns one user, and which view class answers each?


# POST Endpoints: Creating Records

A **POST** request creates a new row in a table. The client sends the new record as JSON in the request **body**, and the API checks it before saving it.

Follow [Django - POST Endpoints](https://ensign.instructure.com/courses/27288/pages/django-post-endpoints).

## Where it goes

A `POST` goes in the view class that already returns the whole table, such as `WebUserAPIView`. The URL `users/` stays the same: Django sends `GET` requests to the `get` method and `POST` requests to the `post` method. You do not change `urls.py`.

## Demo and your turn

0.  In `views.py`, add `from datetime import datetime` and `from rest_framework import status` to the imports
0.  Add a `post` method to `WebUserAPIView`, as shown on the page. It sets `created_date` and `is_active` on the server, then passes `request.data` to a serializer
0.  The serializer checks the data with `is_valid()`. Valid data is saved and sent back. Invalid data gets **400 Bad Request** and a list of errors
0.  Creating a user needs a `password`, but your Week 2 serializer leaves it out on purpose. The page uses a second serializer, `WebUserSerializerPost`, for creating users
0.  Add the `@swagger_auto_schema(operation_description="Add New User", request_body=...)` line above `post` so Swagger knows what body to expect
0.  On `/swagger/`, open `POST /users/`, click **Try it out**, edit the JSON body, and click **Execute**

Send the body as JSON. On the Django page at `/users/`, use the **Raw data** tab with media type `application/json`. In Postman, choose **Body**, then **raw**, then **JSON**.


## Break it on purpose

Send a new user with no `email`. Read the 400 response. Which field does it name?


## Check the database

Confirm the new row with `psql`:

0.  `docker exec -it container_cs415_db bash`
0.  `psql -U postgres`
0.  `\c cs415`
0.  `select * from webuser order by web_user_id desc limit 3;`


## Repeat for every table

Add a `post()` method to the whole-table views for `Addresstype`, `Useraddress`, `Phonetype`, `Userphone`, `Userinfo`, and `Pagedata`.

*Check:* You `POST` a user with no email. What status code comes back, and which part of your code produced the error message?


# PATCH Endpoints: Updating Records

A **PATCH** request changes some fields of a record that already exists. The client sends only the fields to change. Everything else stays as it was, and the record keeps its ID.

Follow [Django - PATCH Endpoints](https://ensign.instructure.com/courses/27288/modules/items/3250476).

## Where it goes

A `PATCH` must know which record to change, so it goes in the view class that takes an ID, such as `SingleWebUserAPIView`. The pattern `users/user/<int:web_user_id>` already exists, so you do not change `urls.py`.

## Demo and your turn

0.  Add a `patch` method to `SingleWebUserAPIView` that takes `self`, `request`, and `web_user_id`, as shown on the page
0.  It looks up the existing record, then builds the serializer from that record, `data=request.data`, and `partial=True`
0.  Add the `@swagger_auto_schema(operation_description="Update WebUser", request_body=WebUserSerializer)` line above it
0.  On `/swagger/`, open `PATCH /users/user/{web_user_id}`, click **Try it out**, enter an ID that exists, set the body to `{"last_name": "Patch"}`, and click **Execute**
0.  Check the row in `psql`: `select * from webuser where web_user_id = 1;` (use your own ID)


## Break it on purpose

Remove `partial=True`, save, and send the same request. Read the response. Then put `partial=True` back.


## `PATCH` or `PUT`?

A `PUT` request replaces the whole record, so the client must send every field. A `PATCH` sends only what parts need to change. Therefore, we will build `PATCH` into our app.


## Repeat for every table

Add a `patch()` method to the filtered view for each of the other six tables.

*Check:* What happens if you leave out `partial=True` and send only `last_name`? Why?


# Work Time

Use the rest of class on your assignments while I step out:

0.  Finish `POST` and `PATCH` endpoints for all seven tables
0.  Finish your [API Documentation using Swagger](https://ensign.instructure.com/courses/27288/modules/items/3250478) screenshots: the users, addresses, and pages sections of `/swagger/`, each showing `GET`, `POST`, and `PATCH`. Your API may run locally with `python manage.py runserver`
0.  Finish your [Ponder and Respond #3 - Faith in Coding](https://ensign.instructure.com/courses/27288/modules/items/3250479)


Push your work to GitHub as you go. If you finish early, help a classmate who is stuck.


# Submitting Today's Exit Ticket

Part of your grade in this class depends upon your attendance. You will help me record your attendance by following these steps:

0.  Find the announcement in our Canvas course titled Module3/Lec06-Wed_Nov_11
1.  Read it and pick one of the questions to respond to
2.  Post a response to the announcement


## Exit Ticket Questions

Pick **one** to answer in your Canvas reply:

0.  What is the difference between `POST` and `PATCH`, and which view class holds each one?
0.  What have you `POST`ed or `PATCH`ed so far, and how did you confirm the change reached the database?
0.  What is one thing about `POST` or `PATCH` that you are still unsure about?


# Wrap Up & Parting Thoughts

## Due Saturday, November 14

0.  [API Documentation using Swagger](https://ensign.instructure.com/courses/27288/modules/items/3250478): upload three screenshots of `/swagger/` from your browser, one each for the users, addresses, and pages sections, showing `GET`, `POST`, and `PATCH`. Your API may run locally; it does not need to run in Docker yet
0.  [Ponder and Respond #3 - Faith in Coding](https://ensign.instructure.com/courses/27288/modules/items/3250479): answer all four questions thoughtfully, with specific examples from your own work. The Ponder and Respond assignments together are about a quarter of your course grade


## Keep your work

Push your `cs415_db` and `cs415_api` repositories to GitHub. Next week builds directly on both.


## What is next

Next week your database moves to AWS and your API moves into a container. That work leads to the Mid-Term Assignment, so come with this week's API working. Watch Canvas for updates.


