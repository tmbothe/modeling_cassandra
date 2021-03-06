# Data Modeling with Cassandra

## Project Description

A compagny that has been collecting songs' data and users activity wants an efficient model to store and analyze the data. The data currently resides in folders in CSV format.
This project consists of creating a database and build an ETL pipeline to store the data.
This will help the analytics to analyze and derive insights from data.

## Data description
The dataset is a set of CSV files in folders partitioned by date. Here is an example:

 - **event_data/2018-11-08-events.csv**
 - **event_data/2018-11-09-events.csv**

The project consist of extracting data from the CSV and load in a cassandra table to answers the questions below:

1- Give me the artist, song title and songs's length in the music app history that was heard during sessionId = 338 and itemInsession = 4
2- Give me only the following: name of artist, song(sorted by itemInSession) and user (firt and last name for userid =10, sessionid = 182
3- Give me every user name(first and last) in my music app history who listened to the songs 'All Hands Againgst His Own'

## Table Modeling
We will create a table with the structure below to answer the questions above:

CREATE TABLE IF NOT EXISTS artist_song (
    artist text,
    first_name text,
    gender text,
    item_in_sessionid int,
    last_name text,
    level text,
    length float,
    location text,
    sessionid int,
    song text,
    userid int,
    PRIMARY KEY(sessionid,item_in_sessionid,userid,song)
    )
  In cassandra, the data can be access efficiently with appropriate primary key. Our primary key is made of:
  - `sessionid` : which is the partition key
  - `item_in_sessionid,userid,song` : which are the clustering columns
  
  ## Queries to answer the questions:
  1- `SELECT * FROM artist_song WHERE sessionid = 338 and item_in_sessionid = 4`
  
  2- `SELECT * FROM artist_song WHERE sessionid = 182 and userid = 10 ORDER BY item_in_sessionid ALLOW FILTERING `
  
  3- `SELECT * FROM artist_song WHERE song='All Hands Against His Own' ALLOW FILTERING ;`
  
    

