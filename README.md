# Firebase Sample App

Testing Google's firebase.  

* This app is deployed at https://app1-6affb.firebaseapp.com/
* The `bigben` endpoint can be accessed at https://app1-6affb.firebaseapp.com/bigben

## What I did

### Initial Setup

```
$ npm install -g firebase-tools

$ firebase login

$ firebase init

```

### Hosting

Create firebase.json file as described in [https://firebase.google.com/docs/hosting/deploying](https://firebase.google.com/docs/hosting/deploying):

```
{
  "hosting": {
    "public": "app",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ]
  }
}
```

Create a simple home page:

```
$ mkdir public

$ echo "hello" > public/index.html
```

### Functions

Initialize functions as decribed in [https://firebase.google.com/docs/functions/get-started](https://firebase.google.com/docs/functions/get-started).    I picked JavaScript.

```
$ firebase init functions
```

It created a `functions` directory.  I then updated the `functions/index.js` with the bigben function as described in [https://firebase.google.com/docs/hosting/functions](https://firebase.google.com/docs/hosting/functions):

```javascript
const functions = require('firebase-functions');

exports.bigben = functions.https.onRequest((req, res) => {
  const hours = (new Date().getHours() % 12) + 1 // london is UTC + 1hr;
  res.status(200).send(`<!doctype html>
    <head>
      <title>Time</title>
    </head>
    <body>
      ${'BONG '.repeat(hours)}
    </body>
  </html>`);
});
```

Also updated firebase.json file with the rewrite rule:

```
   "rewrites": [ {
      "source": "/bigben", "function": "bigben"
    } ]
```

Then, just deploy.  Note that this step took a long time (in minutes).

```
$ firebase deploy
```
