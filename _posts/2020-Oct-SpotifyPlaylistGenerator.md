---
layout: page
title: "Spotify Playlist Generator in Python"
subtitle: "Find out how I made it"
date:   2020-10-20 21:21:21 +0530
categories: ["projects"]
author: "Katherine Moffat"
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

## Getting songs using GET request

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


## Query genres

To get the available genres for seeds

[https://developer.spotify.com/console/get-available-genre-seeds/](https://developer.spotify.com/console/get-available-genre-seeds/)

```
response_genres =requests.get("https://api.spotify.com/v1/recommendations/available-genre-seeds",
               headers={"Content-Type":"application/json",
                        "Authorization":f"Bearer {access_token}"})
print(response_genres.json())
```

### Create the playlist

We will first create an empty playlist in our Spotify account

**User ID**

```python
user_id = "" # YOUR USERID or user URI in a string
```

This is your Spotify user login username.

Alternatively, grab your user URI by going to your profile, clicking on the three dots, then share, then 'Copy user URI'.

**Endpoint URL** 

Our base URL to generate new playlists is:

```python
endpoint_url = f"https://api.spotify.com/v1/users/{user_id}/playlists"
```

You can overwrite the previous endpoint URL, or you can make a new one for clarity. BUT make sure you use the right variable name later on!

**Generate your playlist**

The request to generate your playlist is as follows:

```python
request_body = json.dumps({
          "name": "NAME OF YOUR PLAYLIST",
          "description": "DESCRIPTION OF YOUR PLAYLIST",
          "public": False # let's keep it between us - for now
        })
```

Send the **HTTP POST** request as follows:

```python
response = requests.post(url = endpoint_url, data = request_body, headers={"Content-Type":"application/json", "Authorization":"Bearer " + access_token})
print(response.status_code)
```

Print your response to check the response code. 

Remember:

**201 = OK**

**400 = bad request (maybe a syntax issue?)**

**403 = forbidden (maybe a token issue)**

You can also refresh your Spotify and check whether your playlist appears in your list of playlist 🙂

### Add songs to the playlist

Let's fill our playlist up with the recommended songs we retrieved in the previous session!

**Playlist ID**

From the response of the HTTP POST request to generate the playlist, get the 'ID' of the newly created playlist.

**Note:** The response is an object in the following format:

```python
{
	'attribute': 'value'
	...
	'id' : 'playlist_id'
}
```

To check what other information ('attributes') came back from the request, you can use the **dir(object)** inbuilt function in Python.

To access your playlist ID, access the 'id' attribute in the object as follows:

```python
playlist_id = response.json()['id']
```

**Endpoint URL** 

Our base URL to fill a playlists is:

```python
endpoint_url = f"https://api.spotify.com/v1/playlists/{playlist_id}/tracks"
```

Again, you can overwrite the previous endpoint URL, or you can make a new one for clarity. BUT make sure you use the right variable name later on!

**Playlist ID**

From the response of the HTTP POST request to generate the playlist, get the 'ID' of the newly created playlist.

**Note:** The response is an object in the following format:

```python
{
	'attribute': 'value'
	...
	'id' : 'playlist_id'
}
```

To check what other information ('attributes') came back from the request, you can use the **dir(object)** inbuilt function in Python.

To access your playlist ID, access the 'id' attribute in the object as follows:

```python
playlist_id = response.json()['id']
```

**Send the HTTP POST request**

Remember from the last session how we stored the all the URI's of the songs that came back from the server in a list called 'uris'?

We will now send this list to the server, and 'ask' it to put all those songs into our new playlist, using another HTTP POST request as follows:

```python
request_body = json.dumps({
          "uris" : uris
        })
response = requests.post(url = endpoint_url, data = request_body, headers={"Content-Type":"application/json", "Authorization":f"Bearer {access_token}"})
print(response.status_code)
```

Now your playlist is filled with songs 😆

## ADDITIONAL RESOURCES

**The Article:**

[https://medium.com/analytics-vidhya/build-your-own-playlist-generator-with-spotifys-api-in-python-ceb883938ce4](https://medium.com/analytics-vidhya/build-your-own-playlist-generator-with-spotifys-api-in-python-ceb883938ce4) 

