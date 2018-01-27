# Around_Bloom

A full stack web service for users to login and post images and messages as well as view posts nearby or in designated area.
(Can be deployed to GAE)

Handle four kinds of requests
1. /post 

Post type:
```
Post {
  User String // username
  Message String // post message
  Location { // geo location
    lat
    lon
  }
  Url String // image url
}
```
After receive a post, check user authorization info with JWT.
Post will be saved to ElasticSearch (GCE) and/or BigTable(commented).
The image will be save to GCS.

2. /search

Check user authorization info with JWT.
Search nearby posts by fetching geo location from either browser or ip address.
Search memcache(Redis) first. If it's a miss, search ElasticSearch engine.
Filter the posts with spam check (TODO) and return as response.

3. /signup

Get username and password from request.
Check ElasticSearch if username is already in it.
If not, create a new User object and save to ElasticSearch.
Otherwise, return "user exists" message and 500 code.

4. /login

Get username and password from request.
Check ElasticSearch if username is already in it and if password is correct.
If so, create a token by JWT and return.
Otherwise, return "invalid username or password" message and 500 code.
