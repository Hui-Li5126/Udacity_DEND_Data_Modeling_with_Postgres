# Project objective:
This project is to create a Postgres database for a startup company called Sparkify to falicity analytics team to perforam analyses, particular interest is to know what songs users are listening to. Currently data resides in two directories in JSON format.

# Description:

## What is this project about?
  This project is to design a relational database to store song users information to allow analytics team to perforam joins tables and support their reporting needs.
  
## Database schema design:
  The particular interest is to know what songs users are listening to, to reduce redundancy, ensure data accuracy and integrity, we'd like to divide data into the following tables:
 - song table with song_id as primary key and other info about song such as song name, artist name (we will use artist id here to ensure consistency since artist's name might not be unique enough to be a good identifier), the year the song was released and how long it is.
 - artist table with artist_id as primary key and other data on the artist, such as artist name, location, latitude and longitude 
 - user table with user_id as primary key, along with other info, such as user's first name, last name, gender and level
 - time table uses song play time as primary key, then extract hour, day, week, month, year and weekday from it to allow easy users to slice and dice data with more ease.
 - finially the fact table, uses postGreSQL automatically generated serial number as primary key, along with info from log source data, such as user info, song info, session_id, location, user_agent and play start time.
 
  With those four dimension tables and the fact table, we could join and utilize all info from source data and slice and dice data with different dimensions to answer different business questions.
  
  With this design the update and maintenance is easy and only needed in one table in some cases, such as in case we found an error on artist name, then we only need to update artist name in artist table, when joined with fact table, the artist name would be corrected whenever his/her name shows up, this leads to data consistency. We also reduced redundacy with this design, such as user's first name, last name, gender and level repeatedly show up in log data, which takes up space, with this database schema, we removed all those info to user table, where each user only shows up once.

## Source data:
   Currently song data and log data are stored in two directories in JSON format.
   Below is what song data looks like:
   _{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}_
   Below is what log data looks like:
   _{"artist": "None", "auth": "Logged in", "firstName": "Celeste", "gender":"F", "itemInSession": 0, "lastName":"Williams", "length": null, "level": "free", "location": "Klamath Falls, OR", "method":"GET", "page":"Home", "registration": 1541078000, "sessionId":438, "song": null, "status": 200, "ts": 1541990217796, "userAgent": "Mozilla/5.0(Windows NT 6.1;WOW64)", "userId": 53}_
   
## Clean process:
   Song data is stored with song_id as a unique identifier, duplications on artists, users, times data are dropped to comply the primary key constraint.
   song_id and artist_id are extracted from log data and added to songplays table.
   
   Output tables include below columns respectively:
    **songplays:** songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
    **users:** user_id, first_name, last_name, gender, level
    **songs:** song_id, title, artist_id, year, duration
    **artists:** artist_id, name, location, lattitude, longitude
    **time:** start_time, hour, day, week, month, year, weekday


# How to run the python scripts:
First run **sql_queries.py**, then **create_tables.py**, then **etl.py**

# Python scripts:
**sql_queries.py** includes:
1. drop table syntax in case it already exists, 
2. and create table scripts to create the four dimension tables and one fact table
3. insert rows to those five tables
4. a special song select to get columns for the fact table
5. a query list to allow further iterate through all creation syntax

**create_tables.py** includes:
1. create database
2. drop tables
3. create tables

**etl.py** is the pipeline to read and process json files and insert them into designated tables.

Final output comprises one fact table called songplays, four dimension tables **named, users, songs, artists and time**.
