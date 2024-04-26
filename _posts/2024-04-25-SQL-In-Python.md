# The Power of SQL

As part of Assignment 2 for the *ELEC4630: Computer Vision & Deep Learning* course I am currently taking, I needed to build a Graphical User Interface (GUI) for a fingerprint recognition jupyter notebook.

## The Requirements

The key requirements of the task are as follows:

- The GUI must be able to enroll fingerprints into a database
- The GUI must be able to perform the comparison between a database of fingerprint templates which have been added

## Implementing the Database (DB)

To implement the fingerprint database, I was initially thinking of just using a standard file-system based database. I had done this many times in the past but just felt as though it was not the best way to solve this problem. After some thought, I remembered a previous project I worked a number of years ago where I created a local SQLite database and accessed it using python. I then immediately decided that this was how I would implement the database for the fingerprint GUI.

### Creating A Local Database in Python

I wrote the following code to connect to a sqlite database called `image_gallery.db`. The `sqlite3` library is used to interface with the database throughout the code. The `cursor` object allows you to interact with the database and execute SQL queries. In this sample I have written a query to simply add a table called `images` upon creating the database. The table contains an id (which acts as a primary key), an image `name`, a BLOB containing the `image_data` and then two BLOBs containing the numpy arrays corresponding to the image template properties `minutiae` and `local_structures`.

```python
# Setup DB
# Connect to SQLite database
database_exists = os.path.exists('image_gallery.db')
conn = sqlite3.connect('image_gallery.db')
cursor = conn.cursor()
if not database_exists:
    cursor.execute('''CREATE TABLE images
                      (id INTEGER PRIMARY KEY AUTOINCREMENT,
                       name TEXT NOT NULL,
                       image_data BLOB NOT NULL,
                       minutiae BLOB NOT NULL,
                       local_structures BLOB NOT NULL)''')
```

### Interacting with the Database

Since this is a sql database, SQL queries are executed to interact with the database, this includes adding/removing/updating data within the tables.

An example of adding a fingerprint image to the database can be seen below.

```python
cursor.execute('''INSERT INTO images (name, image_data, minutia, local_structures) VALUES (?, ?, ?, ?)''', (name, imageData, minutiaBytes, localStructuresBytes))
conn.commit()
```

Overall, interfacing with the SQLite database in python was fairly seamless and fast. I would encourage others to also attempt implementing an SQLite database in python as a good exercise to learn SQL if you are not already familiar with it.
