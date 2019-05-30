---
layout: single
title:  "Manage environment variables with dotenv in Node.js applications"
date:   2019-05-29 16:45:56 -0700
categories: 
    - Development Notes
tags:
    - dotenv
    - environment variable
    - Node.js
classes: wide
---

Last Edited: May 29, 2019 5:09 PM

I was recently writing a Node.js web application in WebStorm. A part of the local development was creating and managing environment variables and prevented them from storing in the source control (e.g. Git). Here are some notes of the process I followed:


Install the `dotenv` package

```bash
    # use npm
    npm install dotenv
```


Require and configure dotenv at the very top of the root `app.js` file

```javascript
    require('dotenv').config();
```


Create a `.env` file in the root directory of the project, and add environment variables in the format `NAME=VALUE`. For example, in my project, I added the keys below. Note that no quotation marks needed for the value if there is no space between words:

```javascript
    DB_URL=mongodb://localhost:27017/[put the project name]
    GEOCODER_API_KEY=[Put the secret key here]
```


Now `process.env` can access the environment variables (keys and values) defined in `.env`. For example, I used MongoDB in this project and connected mongoose with `DB_URL`:

```javascript
    mongoose.connect(process.env.DB_URL, {
      useNewUrlParser: true,
      useCreateIndex: true
    }).then(() => {
      console.log('Connected to DB');
    }).catch(err => {
      console.log('ERROR:', err.message);
    });
```

I also set up Google Maps API with the key `GEOCODER_API_KEY` and used `geocoder` in the later code:

```javascript
    var options = {
        provider: 'google',
        httpAdapter: 'https',
        apiKey: process.env.GEOCODER_API_KEY,
        formatter: null
    };
    var geocoder = NodeGeocoder(options);
```


On the same level of `.env`, you may want to add `.gitignore` file to intentionally specify all directories and files that you want Git to ignore so that they are not exposed on your repo. Mine looks like this:

```javascript
    # Ignore files related to API keys
    .env
    
    # Ignore node_modules folder
    node_modules/
    
    # Ignore Mac system files
    .DS_store
```


If the app is hosted remotely: define all the environment variables everywhere where the app is hosted/started. In my case, I added the env variables to heroku where my app is hosted (note: here heroku is already set up):

```bash
    $ cd my app
    $ heroku config:set KEY=VALUE    # e.g. GITHUB_USERNAME=JANEDOE
```

- You can view all current variables or remove any variable:

```bash
    $ heroku config    # View all variables
    GITHUB_USERNAME: JANEDOE
    
    heroku config:get GITHUB_USERNAME    # Check value to the key
    JANEDOE
```

DONE!

## References
1. [Configuration and Config Vars](https://devcenter.heroku.com/articles/config-vars)
2. [dotenv Setup](https://github.com/motdotla/dotenv)