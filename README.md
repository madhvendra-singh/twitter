# Twitter Bot
Thanks To TumkurLab and Yathin Babu

Follow: https://twitter.com/madhvendrax

## Features

This is a simple Twitter bot and will retweet, favorite/like randomly on the basis of hashtags as a query that we will use.

## Requirements

* You must have [Node.js](https://nodejs.org) installed on your system.
* A Twitter Account.
* Your bot will be using [twit](https://www.npm.im/twit) which is an npm module to manipulate tweets and streams, and to communicate with the [Twitter API](https://developer.twitter.com/en/docs).
* A [Heroku](https://www.heroku.com) free account to deploy your bot.
* Install [git](https://git-scm.com/downloads).

## Setup

Setup an empty directory and initialise it with:$ npm init to configure this web application with package.json file. Then create two new files: bot.js & config.js in that directory.

bot.js will be our main app file in which we will be writing the source code of our Twitter Bot, and so in package.json edit the main field to:
```js
{  
  "main": "bot.js",  
},
```
Your current directory structure should look like this:

root/project-name
|- bot.js
|- config.js
|- package.json

### Configuring and granting permissions from Twitter API

After logging to to your Twitter account, follow to this link: https://developer.twitter.com/en/apps to create a new application. Fill out the necessary fields in the form click on the button Create Your Twitter Application. After creating the application, look for ‘Keys and Access Tokens’ under the nav-panes and click on 'Generate Token Actions' and then copy:

* Consumer Key
* Consumer Secret
* Access Token
* Access Token Secret

Open the config.js file and paste all four values inside it. Expose those values using module.export:
```js
//config.js
 module.exports = {
  consumer_key: '',  
  consumer_secret: '',
  access_token: '',  
  access_token_secret: ''
}
```

Now, the Twitter bot’s configuration is step is complete. Please note, for every different application, the consumer key, consumer secret, access_token and access_token_secret will differ.

## Building the bot

Since the configuration step is complete, now let’s install our third requisite that is Twitter API client for node and will help us to communicate to Twitter API and provide an API for all necessary actions (such as retweet and favorite a tweet).

We will start by installing the dependency we need for our application.
```sh
$ npm i twit
```
After the dependency has finished installing, go to the bot.js file and require the dependency and config.js file.
```js
const twit = require(`twit`);
const config = require(`./config.js`);
```
Pass the configuration (consumer and access tokens) of our Twitter application in config.js to twit:
```js
const Twitter = new twit(config);
```
PLEASE NOTE: You must refer to twit documentation for a deep reference.

## Retweet Bot

Let’s write a function expression that finds the latest tweets according to the query passed as a parameter. We will initialise a params object that will hold various properties to search a tweet, but most importantly query or q property that will refine our searches. Whatever value you feed in this property, our bot will search the tweets to retweet based on this criteria. You can feed this property values like a twitter handler, to monitor a specific twitter account or a #hashtag. For our example bot, we have find latest tweets on #nodejs.

This is how the functionality of the retweet bot starts:
```js
function retweet() {
  const params = {
    q: '#nodejs, #Nodejs',
    result_type: 'recent',
    lang: 'en'    
  } 
```
The other two properties: result_type and lang are optional. On defining the result_type: 'recent' notifies bot to only search for the latest tweets, tweets that have occurred in the time period since our bot has started or it made the last retweet.

Our next step is to search for the tweets based on our parameters. For this, we will use Twitter.get function provided by twit API to GET any of the REST API endpoints. The REST API endpoint is a reference to the Twitter API endpoint we are going to make a call to search for tweets. The Twitter.get function accepts three arguments: API endpoint, params object (defined by us) and a callback

To post or to retweet the tweet our bot has found we use Twitter.post() method to POST any of the REST API endpoints. It also takes the same number of arguments as `Twitter.get()`.

Now to automate this action we defined above, we can use JavaScript’s timer function setInterval() to search and retweet after a specific period of time.
```js
// grab & retweet as soon as program is running...
retweet();
// retweet in every 50 minutes
setInterval(retweet, 3000 * 1000); //3000 sec
```
Please note that all JavaScript’s Timer functions take the amount of time argument in milliseconds.

## Favorite Bot

Similar to retweet bot we can define and initialise another function expression that will search and favorite a tweet randomly. Yes, the difference here is to search and grab the tweet randomly. We will start by creating a parameter object params that will consist of three properties as in `retweet()` function expression. The bot will search for tweets using the same `.get()` function provided by twit API to GET any of the Twitter API endpoints. In our case, we need search/tweets. Then we will store the status of the search for tweet to favorite in a variable and in a another variable we will apply the random function by passing the “status of the search” variable as an argument.

Note that the tweets searched by our bot are all stored in an array. Again, we use JavaScript’s timer function `setInterval()` to search and favorite the tweet after a specific period of time in milliseconds.

## Usage

To run this bot, go to your terminal:
```sh
$ node bot.js
```
To avoid this monotonous process you can use npm scripts or nodemon. You can also deploy this app on Heroku for a continuous integration.

To use npm scripts, make this edit under scripts in package.json :
```json
{
  "scripts": {    
    "start": "node bot.js",  
  }
}
```
Then from terminal:
```sh
$ npm start
```

## Deploy on heroku

* Signup for a free account by filling a form on https://heroku.com .

* After a successful signup, you will be redirected to dashboard.

* Create a new app by clicking the New button at the top right.

* Fill in your app name (it should be in lower case) and choose your relevant region.

* Move to the directory where you have saved the bot source file using CLI.

* Download Heroku CLI. Refer the document here on how:

https://devcenter.heroku.com/categories/command-line

Install it by using Teminal on Mac or Windows Powershell:
```sh
$heroku
```
* Login through CLI:
```sh
$heroku login
```
Give the credentials. You will be logged in.

* Choose your **Deployment method** on the dashboard, it could be Heroku Git, GitHub or Dropbox. But I highly recommend to go with Heroku Git.

* Follow the cammands given under deployment methods to turn your directory into a git repository and to deploy on heroku.

* Make sure the app is worker app not a web app. Procfile is necessary for that.
