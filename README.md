# Universal TV APP

**⚠️ This repositary contains only documentation, to how your external API output should look like, so your service will be compatible with our app.**

## How to handle easy installation for user

**App is listening to url handler starting with universaltv://**

- To setup user's account in one click, just add to your site setup button, which will contain our URI Handler with 2 parameters to specify your API URI, and user's token to identify account
- Token cannot contains string "/", as it would interrupt splitting parameter of token and parameter of your API URI
- Token will be in any request to your API as parameter under key Token

example: universaltv://setup_account/1Ebvr4Q20MWvZgSi/http://api.mytv.com

- If deeplink is not working properly, user can always simply just enter his token & your API URI to the form, which is shown if he does not have setup his account in app.

example:
API: http://api.mytv.com
TOKEN: 1Ebvr4Q20MWvZgSi

## How to handle user's authetication in any request
- All requests to your API will contain parameter of user's token under key token
example of request to handle user's channels under page 0:
POST REQUEST to URI: http://api.mytv.com/getChannels/0
POST PARAMETERS: {
  "token": "1Ebvr4Q20MWvZgSi",
}

## SAMPLE API OUTPUTS
**⚠️ All of your server outputs must be valid as defined below**
**⚠️ Be aware of any error reporting on your server's API, once you will have enabled any error reporting, it may return damaged JSON output, and APP won't be able to handle requests to your API properly.**

1. List of channels
**⚠️ - List of channels handler need to be under subpage "/getChannels/[PAGE]"**
- You need to be able to proccess paging
- Type: POST
- REQUEST POST PARAMETERS:
```js
{
  "token": "[TOKEN]",

}
```

- REQUEST RETURN:
```js
{
  "channels": {
    "ch0": {
      "id": 1, //id of your channel, for example as it is also in DB
      "name": "BBC 1", //set the name of channel
      "logo": "http://www.electricpig.co.uk/wp-content/uploads/2008/06/bbc-one.gif", //URI to your channels logo (width & height should be ration 1:1, for example 320x320, 640x640)
      "runningAvailable": true, //true if you will return current running & next running program. false if channel does not have EPG available!
      "running": {
        0: { //current running program must be under value 0
          "from": 1557001570, //unix timestamp of from what time current program is running
          "to": 1557021570, //unix timestamp of to what time current program is running
          "name": "Harry Potter and the Sorcerer's Stone", //name of current program
        },
        1: { //next running program must be under value 1
          "from": 1557021570, //unix timestamp of from what time next program is running
          "to": 1557041570, //unix timestamp of to what time next program is running
          "name": "Harry Potter and the Chamber of Secrets", //name of next program
        }
      }
    },
    "ch1": {
      "id": 2, //id of your channel, for example as it is also in DB
      "name": "BBC 2", //set the name of channel
      "logo": "https://pbs.twimg.com/profile_images/1057914504321908736/06hkWvx__400x400.jpg", //URI to your channels logo (width & height should be ration 1:1, for example 320x320, 640x640)
      "runningAvailable": false, //true if you will return current running & next running program. false if channel does not have EPG available!
    },
  },
  "isLast": false
}
```

2. Channel details

3. Add/Remove channel to favourite

4. Report problem with channel

5. News

6. Account data

7. Save push token
**⚠️ - Save push token handler need to be under subpage "/pushToken"**
- Type: POST
- REQUEST POST PARAMETERS:
```js
{
  "token": "[TOKEN]", //user's auth token
  "pushToken": "[EXPO_TOKEN]",

}
```
- You should save user's token, to user. Later you can send him simply notification as it is covered under documentation of Expo Push Notifications
https://docs.expo.io/versions/latest/guides/push-notifications/


8. Translations
- Output from your API Server must need just simple JSON Object containing all translation strings included also in this github at /translation/strings.json
- Each language strings must be defined under itself ISO 639-1 code.
https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
Example:
```js
{
  "en": {
    "_lang": "cs",
    "hello": "Hello",
    "goodMorning": "Good morning",
  },
  "de": {
    "_lang": "cs",
    "hello": "Hallo",
    "goodMorning": "Guten Morgen",
  }
}
```
