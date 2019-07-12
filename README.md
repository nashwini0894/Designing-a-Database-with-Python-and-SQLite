# Designing-a-Database-with-Python-and-SQLite
## Description
Application to read an iTunes export file in XML and produce a properly normalized database. The python code to derives track name, 
artist details, album details, genre and other details of the track from the XML file. Uses these details to create tables for Artist
details, genre details, Album details and Track details in a database.

## About the Input
The XML file used for this application is an iTunes export file. This file has a collection of tracks from various albums, their details 
such as artist name, genre, number of times played, length of the track, etc. For this application, we are interested in obtaining the
following details from the XML file:
1. Name of Track
2. Name of Artist
3. Genre of the track
4. Length of the track
5. User rating
6. Number of times the track was played

## About the Code
We import xml.etree.ElementTree and sqlite3 to read the XML and create a SQLite database respectively. In the modelling of a database the basic rule is not to put in same string twice, instead use a relationship. Repating of strings consumes unnecessary space and also making scaning through the database difficult.

In order to create our database, we first to need to figure out what are our objects. In our case that would be the 6 specifications we are obtaining from the XML file. Once, our objects are defined, we define the relationship between these objects. So we try making different tables to avoid replicating data. Instead, we can store an ineteger value for them. As we can see, genre, artist name and album name have replicating values. So, we make them as different fields and assign an integer each time a new genre, artist or album name is assigned to the table. These numbers could be later associated with the tracks in the track table. Other factors such as the length of the track, user rating and number of times played could be directly associated with the track as they are solely dependent on the track.

The table structure for each of these tables would be:

1. Artist Table:
CREATE TABLE Artist (
    id  INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    name    TEXT UNIQUE
);

2. Genre Table:
CREATE TABLE Genre (
    id  INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    name    TEXT UNIQUE
);

3. Album Table:
CREATE TABLE Album (
    id  INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
    artist_id  INTEGER,
    title   TEXT UNIQUE
);

5. Track Table:
CREATE TABLE Track (
    id  INTEGER NOT NULL PRIMARY KEY 
        AUTOINCREMENT UNIQUE,
    title TEXT  UNIQUE,
    album_id  INTEGER,
    genre_id  INTEGER,
    len INTEGER, rating INTEGER, count INTEGER
);

The lookup(d, key) function, retrives data from the XML file where the lines are as follows:
<key>Track ID</key><integer>369</integer>
<key>Name</key><string>Another One Bites The Dust</string>
<key>Artist</key><string>Queen</string>

The XML files is a list of dictionaries so we parse through the file to find the track details. Once all the required details are obtained they are inserted into their respective tables. This would create the required database, we could then run our set of SQL command in SQLite to derive the information we want from the database by performing relevant join operations.

## About the Output
The output generated as a result of this python code is an SQLite database containing the following tables:
1. Track Table,
2. Artist Table,
3. Genre Table, and
4. Album Table.
We coukd load this databse in the SQLite browser and execute our SQL commands
For example: 
SELECT Track.title, Artist.name, Album.title, Genre.name 
    FROM Track JOIN Genre JOIN Album JOIN Artist 
    ON Track.genre_id = Genre.ID and Track.album_id = Album.id 
        AND Album.artist_id = Artist.id
    ORDER BY Artist.name LIMIT 3
would generate the following ouput, as attached in the ouput.
