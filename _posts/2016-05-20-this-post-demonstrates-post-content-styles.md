---
layout: page
title: "Spotify Playlist Generator in Python"
subtitle: "Find out how I made it"
date:   2020-10-20 21:21:21 +0530
categories: project
author: "Katherine Moffat"
meta: "Springfield"
---

# Contents
Here I have outlined how I created a Python script that generates playlists in Spotify!

Using 'Seeds' to generate specific playlists as well as user top artist and song data to find new songs that you may like



## Installing 'requests' library

Install requests library from the command line:

'''
python3.7 -m pip install requests** 
'''
Replace 'python3.7' with whatever python version you have.


## Spotify API

[https://developer.spotify.com/console/get-recommendations/](https://developer.spotify.com/console/get-recommendations/) 

Scroll to the bottom and click '**Get Token Button**' and click '**playlist-modify-private**' as the scope you need. You will also need to sign in to Spotify. (Note this is a short lived token, for serious application there is another process...I will implement this later??)

**APIs**

APIs (Application Programming Interface) list a bunch of operations that a programmer can use

This is what we are doing in this session

1. an HTTP GET request to the /recommendations endpoint, to get the tracks;

2. an HTTP POST request to the /playlists endpoint to create a new playlist;

3. an HTTP POST request to the /playlist endpoint to add the songs.


# **Let's get started! :)**

# Getting songs using GET request

### Need to import some python libraries!

- Requests library are what allow us to make the get and post requests
- json is a data format;

```
import requests
import json
```

 endpoint_url is our **base URL for making our query** and store your access token

```
endpoint_url = 'https://api.spotify.com/v1/recommendations?'
access_token = "ENTER-YOUR-TOKEN"
```

### Start creating some filters!

This is how we will filter through all the songs to get a more specific music for our playlist

```
#FILTERS
limit = 10 #number of songs
market = "AU" #country
seed_genres = "pop" #genres
target_danceability = 0.2
seed_artists = '06HL4z0CvFAxyc27GXpf02?si=_0xxqWK1RMm-QYK6TwApQg'
```

- **To get the artist id** you need to go to the artists profile on Spotify (Desktop version), go to the three dots, then 'Share', and then 'Copy Spotify URI'

    The URI will come back in this format:

    ```python
    spotify:artist:**6a3Mfrn2XBR1DfPg1QGa1d**
    ```

    You only need the part ***after*** 'spotify:artist:'

- You can also get this from the URL of the artist page if you use Spotify in the browser.
- You can **browse genres on Spotify**. Some examples are Punk, folk, hiphop, R&B, Country etc

We will keep the filters simple for now and can add more later!

### Let's get some songs!

```
#QUERY FOR SONGS
query = f'{endpoint_url}limit={limit}&market={market}&seed_artists={seed_artists}&target_danceability={target_danceability}'
```

Add on the seeds/filters to the query string

```
response = requests.get(query,
               headers={"Content-Type":"application/json",
                        "Authorization":f"Bearer {access_token}"})

```

Print the response and see if it worked!

**200 = OK**

**400 = bad request (maybe a syntax issue?)**

**403 = forbidden (maybe a token issue)**

Now lets store these songs so we can later add them to our playlist

Create an array to store the playlist (mine is 'uris')

```
json_response = response.json()
for i,j in enumerate(json_response['tracks']):
            uris.append(j['uri'])
            print(f"{i+1}) \"{j['name']}\" by {j['artists'][0]['name']}")
```

**This explains all the 'seeds' (filters)**

[https://developer.spotify.com/documentation/web-api/reference/browse/get-recommendations/](https://developer.spotify.com/documentation/web-api/reference/browse/get-recommendations/)

### Thanks for coming!

`Please leave us some feedback and comments on how you thought the session went!`

These sessions are to genuinely help and encourage learning so we want to hear your feedback so we can improve and make them the best we can!

[https://cutt.ly/mac-collab-feedback](https://cutt.ly/mac-collab-feedback)

## Query genres

To get the available genres for seeds

[https://developer.spotify.com/console/get-available-genre-seeds/](https://developer.spotify.com/console/get-available-genre-seeds/)

```
response_genres =requests.get("https://api.spotify.com/v1/recommendations/available-genre-seeds",
               headers={"Content-Type":"application/json",
                        "Authorization":f"Bearer {access_token}"})
print(response_genres.json())
```

## ADDITIONAL RESOURCES

**The Article:**

[https://medium.com/analytics-vidhya/build-your-own-playlist-generator-with-spotifys-api-in-python-ceb883938ce4](https://medium.com/analytics-vidhya/build-your-own-playlist-generator-with-spotifys-api-in-python-ceb883938ce4) 

**The code:**

[https://gist.github.com/rob-med/167bc67169133712ee0e75f76103fbc5](https://gist.github.com/rob-med/167bc67169133712ee0e75f76103fbc5)

[https://nbviewer.jupyter.org/gist/rob-med/167bc67169133712ee0e75f76103fbc5](https://nbviewer.jupyter.org/gist/rob-med/167bc67169133712ee0e75f76103fbc5)
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit.

## Some great heading (h2)

Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu.

Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Another great heading (h2)

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit.

### Some great subheading (h3)

Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum.

Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc.

### Some great subheading (h3)

Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

> This quote will change your life. It will reveal the secrets of the universe, and all the wonders of humanity. Don't misuse it.

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt.

### Some great subheading (h3)

Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum.

```html
<html>
  <head>
  </head>
  <body>
    <p>Hello, World!</p>
  </body>
</html>
```


In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris.

#### You might want a sub-subheading (h4)

In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris.

In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris.

#### But it's probably overkill (h4)

In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris.

### Oh hai, an unordered list!!

In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris.

- First item, yo
- Second item, dawg
- Third item, what what?!
- Fourth item, fo sheezy my neezy

### Oh hai, an ordered list!!

In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris.

1. First item, yo
2. Second item, dawg
3. Third item, what what?!
4. Fourth item, fo sheezy my neezy



## Headings are cool! (h2)

Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc.

### Tables

Title 1               | Title 2               | Title 3               | Title 4
--------------------- | --------------------- | --------------------- | ---------------------
lorem                 | lorem ipsum           | lorem ipsum dolor     | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit  


With uneven columns

Title 1 | Title 2 | Title 3 | Title 4
--- | --- | --- | ---
lorem | lorem ipsum | lorem ipsum dolor | lorem ipsum dolor sit
lorem ipsum dolor sit amet | lorem ipsum dolor sit amet consectetur | lorem ipsum dolor sit amet | lorem ipsum dolor sit
lorem ipsum dolor | lorem ipsum | lorem | lorem ipsum
lorem ipsum dolor | lorem ipsum dolor sit | lorem ipsum dolor sit amet | lorem ipsum dolor sit amet consectetur
