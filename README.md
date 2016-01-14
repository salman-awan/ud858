P4: Conference Organization App
============

AUTHOR
------
Salman Awan


INTRODUCTION
------------
This repository contains the submission for Project 4 of the Udacity Full Stack Devleoper Nanodegree. This project contains code for a conference organization app which uses the Google App Engine (GAE).


REQUIREMENTS
-------------
You will need to install Python 2.7.x and Google App Engine (1.x).


TASK 1: ADD SESSIONS TO CONFERENCE
----------------------------------
Session model object contains properties for session name, highlights, speakers, duration, type of session, date, and start time. Speakers is a list of strings.


TASK 3: ADDITIONAL QUERIES
--------------------------
1. Get Wishlist sessions by speaker
2. Get Wishlist sessions by city



TASK 3: QUERY PROBLEM
---------------------
To query for all nonworkshop sessions before 7 pm, the problem we face is that the Google Datastore does not allow inequality comparisons (<, <=, >, >=, !=) on more than one property in a single query. But for this query, we want to apply a NOT EQUAL TO filter for the session type property as well as a LESS THAN filter for the session start time property.

The most intuitive way to workaround this issue is to apply only one inequality filter and then filter out the unwanted results in Python code. This is the solution I have implemented.

Another possible way to do this might be to change the NOT EQUAL TO filter for session type to a IN filter containing all session type values which are non-workshop. That would do it in one Datastore query.


RUNNING THE APPLICATION
-----------------------
In folder /vagrant/catalog, run the following command:

python application.py

This will start the website which can be accessed with this URL: http://localhost:5000


USAGE
-----
You can use the links in the main catalog page to view the categories and items within those categories.

Only logged in users can add items, and then edit or delete the items they have added.

This project uses Google sign in for authentication. After clicking the Login link, you will need to use your Google account to log in and allow access for this application.

Once logged in, the relevant web pages will allow you to add new items and later edit or delete those items.
