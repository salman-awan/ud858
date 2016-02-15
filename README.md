P4: Conference Organization App
============

AUTHOR
------
Salman Awan (salman.awan@gmail.com)


INTRODUCTION
------------
This repository contains the submission for Project 4 of the Udacity Full Stack Devleoper Nanodegree, which is a conference organization app which uses the Google App Engine (GAE).


REQUIREMENTS
-------------
You will need to install Python 2.7.x and Google App Engine (1.x).


TASK 1: ADD SESSIONS TO CONFERENCE
----------------------------------
For this task, two new classes were added named Session (NDB model) and SessionForm (ProtoRPC message).

The Session class contains properties for session name (string property), highlights (string property), speaker (string property), duration (integer property), type of session (string property), date (date property), and start time (time property). Following are some notable design decisions behind the Session class design:

- The 'speaker' property is implemented as a string property containing the name of the speaker.
- The 'highlights' property is a string.
- The 'duration' is the duration of the session in minutes.
- The 'type' property is a string in the Session class but is represented as a SessionType enum in the corresponding SessionForm ProtoRPC message class. This was done to restrict the session types to known types.
- The 'date' property uses the Datastore Date type to store the date of the session.
- The 'startTime' property uses the Datastore Time type to represent the start time of the session. It is converted to/from 24 hr format when serializing it to SessionForm object

Following are notable design decisions regarding how the different entities interact and work together:

- The SessionForm class contains corresponding properties for all Session class properties. It also includes a 'websafeKey' property which contains the web-safe version of the datastore key for that session object.
- The Session objects all have the respective Conference objects set as their parent in the datastore. This makes it easy to get the parent Conference object when we only have the Session object available.
- The Conference class was modified to add a list of all child sessions. This makes querying for sessions within a conference much easier.


TASK 2: SESSION WISHLIST
------------------------
The endpoint methods to add a session to the current user's wishlist, get a list of all sessions in the user's wishlist, and delete a session from the user's wishlist were implemented as required. The user's wishlist is stored in the existing Profile model class as a property named 'wishlistSessionKeys' which contains a list of session keys (strings) representing the sessions in the user's wishlist.


TASK 3: ADDITIONAL QUERIES
--------------------------
Query # 1. Get Wishlist sessions by speaker: Added a new endpoint method to get the sessions in the current user's wishlist filtered by the specified speaker. This will be useful for first querying the featured speaker (task 4) and then running this query to get all sessions by the featured speaker. It was implemented by filtering the sessions in the wishlist by the specified speaker.

Query # 2. Get Wishlist sessions by city: Added a new endpoint method to get the sessions in the current user's wishlist filtered by the specified city. This will allow the user to quickly see all the sessions they want to attend in a particular city across all conferences. It was implemented by filtering the sessions in the wishlist by the specified city which was obtained via the parent conference object for the sessions.


TASK 3: QUERY PROBLEM
---------------------
To query for all nonworkshop sessions before 7 pm, the problem we face is that the Google Datastore does not allow inequality comparisons (<, <=, >, >=, !=) on more than one property in a single query. But for this query, we want to apply a NOT EQUAL TO filter for the session type property as well as a LESS THAN filter for the session start time property.

The most intuitive way to workaround this issue is to apply only one inequality filter and then filter out the unwanted results in Python code. This is the solution I have implemented.

Another possible way to do this might be to change the NOT EQUAL TO filter for session type to a IN filter containing all session type values which are non-workshop. That would do it in one Datastore query.


TASK 4: FEATURED SPEAKER
---------------------
For this task, the getFeaturedSpeaker endpoint method was added to get the current featured speaker. The featured speaker is stored in the memcache whenever a new session is created and the speaker for that session has more than one sessions. Previous featured speakers are overwritten. Moreover, the memcache entry is added via a background task implemented using the task queue feature in GAE.


RUNNING THE APPLICATION
-----------------------
To start the app on localhost, run the Google App Engine Launcher, then add the GAE project named 'salcon-1145' in the folder: <ud858>\ConferenceCentral_Complete. After starting the project, it will get hosted at: http://localhost:<port>/ where the default value for <port> is typically 8080.

This project has also been deployed to the GAE cloud: https://salcon-1145.appspot.com/


USAGE
-----
Before fully testing the project, you must login to the app using your Google account, then go to My Profile page, and update your Tee shirt size (click Update profile button after that). This will ensure that your profile instance gets saved in the NDB.

After that, you can either use the web UI to use the features, or go to the API explorer to test the newly added endpoints: https://apis-explorer.appspot.com/apis-explorer/?base=https://salcon-1145.appspot.com/_ah/api#p/conference/v1/
