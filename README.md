# Steps for deploying full stack react app.

These steps assume that your project folder structure looks like so:

```
    client/
    server/
```

## 1. Cloud

1. Database: Go to https://cloud.mongodb.com/ and create an database instance of you do not already have one.
2. Heroku: go to https://dashboard.heroku.com/apps and create a new app.
3. In the settings tab add to the config vars MONGO_URI and the connection string with your database that you can find in mongo.

## 2. Client

1. First make copy of your .env.development file.
2. Change the name of the copied file to .env.production.
3. In .env.production change the value REACT_APP_BASE_URL to the URL of your heroku app, which you can find on https://dashboard.heroku.com/apps.

In the client folder add to your package.json under the script property the following two lines.

```
    "build-dev": "dotenv -e .env.development react-scripts build",
    "build-prod": "dotenv -e .env.production react-scripts build",
```

## 3. Server

In app.js make sure you serve static react files:

```
app.use(express.static(path.join(__dirname, "build")));
```

After all your other routes add the folliwing code to always serve your react app.

```
app.get("/*", function(req, res) {
  res.sendFile(path.join(__dirname, "build", "index.html"));
});
```

In your /server file package.json add:

```
"deploy": "cd ../client && npm run build && rsync -r build ../server/ && cd .. && git subtree push --prefix server heroku master"

```

## 4. Deploy

1. Add the location of your heroku repository to git by running the folliwing command:

```
heroku git:remote -a name-of-your-app
```

2. Now everytime you want to deploy, go to /server and run:

```
$ npm run deploy
```
