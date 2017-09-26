# GameStream Docs
GameStream is a rich visualization of the game using NFL NextGen (NGS) data, which tracks every player using sensors in their shoulder pads.  There are both a fully hosted, white-label solution with big plays coming through live (post play), and an embeddable interactive play visualization component.

## Hosted Solution
We provide a fully hosted, white-label solution under your branding, offering a unique and engaging destination for fans.

http://gamestream.sportradar.com/

Customers can customize the solution by:
- URL (host name drives the customer setting for GameStream)
- Logo
- Header Color
- Ad Tags
- Tracking Tags

![image](https://user-images.githubusercontent.com/1287502/28985009-68212b88-7915-11e7-8c6a-edc925b93bc6.png)

### Deep Linking
Each individual item can be deep linked by including the RegistryId on the URL.

`gamestream.sportradar.com/:registry_id=xxx`

EX: http://gamestream.sportradar.com/9b84dc60-7936-11e7-b2dd-f550eadcce76

## Embed Player
An embeddable interactive control to display NFL plays based off NGS player tracking data.

![image](https://user-images.githubusercontent.com/1287502/28947607-a6b74b08-7865-11e7-893c-45e0a557f1b3.png)

- Use an iFrame to embed
- Set Width and Height

_Note: Height varies based on width, need to lock in exact aspect ratio, for now user must find right size._

`http://[customer domain]/embed/:registry_id?auto=true|false&loop=true|false`

Ex: http://gamestream.sportradar.com/embed/9b84dc60-7936-11e7-b2dd-f550eadcce76

- registry_id: [GameStream Registry API](https://github.com/sportradarus/badlands/wiki/GameStream-Registry-API)
- loop: Determines if the player loops the video after it is done
- auto: Determines if the player auto plays or not (defaults to auto play)
- frontSlate: Default is false and can be set to true to show front slate
- endSlate: Default is true (unless loop=true) and can be set to false to remove end slate from showing
- startIndex: Only evaluated when `auto` is set to `false`. Valid values are `first` or `last`. Determines if the thumbnail image for the player is that of the first index or the last.  Defaults to the first.
- camera: Determines which camera to use. Available values are 1,2,3 etc and are based on the camera option placement for the particular play.  For example for a rushing play, the available cameras are 1 (coach) or 2 (rush). 

## Registry API
For deeper integration or customization, Sportradar provides an API to access the items in GameStream or filter the list of registry items.

### /registry
Returns a list of registry items, with paging and filter capability.

https://6h4vl13py3.execute-api.us-west-2.amazonaws.com/prod/registry

#### Filter Options
Use any combination of filters to limit the lists to registry items matching your criteria.

`GET /registry?filterName=[filtervalue|...]&filterName=[filtervalue|...]`

- week
- seasonType (REG, PST)
- season
- classification (auto filtered by BIG_PLAY, no other classifications at this time)
- gsisGameId
- srGameId
- teamId (these are sr (Sportradr) teamids)
- srPlayId
- srWeekId
- srGameId

**EXAMPLES:** (%7C is the encoded |)

https://6h4vl13py3.execute-api.us-west-2.amazonaws.com/prod/registry?week=7&teamId=4984%7C5003
https://6h4vl13py3.execute-api.us-west-2.amazonaws.com/prod/registry?week=3&season=2016&seasonType=REG
https://6h4vl13py3.execute-api.us-west-2.amazonaws.com/prod/registry?srGameId=8c512845-57fb-4947-84d8-76a637358a84

**Example Response:**
```javascript
{
  "nextIndex": "1485131814000",
  "count": 10,
  "items": [
    {
      "id": "ebdc23a0-73e4-11e7-af3f-cf2bde4ab38a",
      "type": "play-breakdown",
      "time": "2017-01-23T02:32:15.000Z",
      "timeMs": "1485138735000",
      "initializationData": {
        "yfd": 10,
        "away": {
          "name": "Steelers",
          "alias": "PIT",
          "gsisId": "5000",
          "market": "Pittsburgh",
          "points": 17,
          "srusId": "cb2f9f1f-ac67-424e-9e72-1475cb0ed398"
        },
        "down": 1,
        "home": {
          "name": "Patriots",
          "alias": "NE",
          "gsisId": "4994",
          "market": "New England",
          "points": 36,
          "srusId": "97354895-8c77-4fd4-a860-32e62ea7382a"
        },
        "clock": "3:42",
        "title": "Touchdown! B. Roethlisberger 30 yd pass to C. Hamilton",
        "period": 4,
        "playType": "pass",
        "yardLine": 30,
        "playClock": 11,
        "awayPoints": 15,
        "homePoints": 36,
        "periodType": "quarter",
        "possession": "5000",
        "description": "(3:42) (Shotgun) 7-B.Roethlisberger pass deep right to 83-C.Hamilton for 30 yards, TOUCHDOWN.",
        "featurePlayers": [
          {
            "name": "Ben Roethlisberger",
            "team": {
              "gsisId": "5000",
              "srusId": "cb2f9f1f-ac67-424e-9e72-1475cb0ed398"
            },
            "gsisId": "00-0022924",
            "number": "07",
            "srusId": "ea357add-1a41-4a8b-8f34-bbfade7f4d98",
            "position": "QB",
            "headshotKey": "89080494-5e5d-5ccd-bf4a-699be236d124",
            "enhancedPosition": "passer"
          },
          {
            "name": "Cobi Hamilton",
            "team": {
              "gsisId": "5000",
              "srusId": "cb2f9f1f-ac67-424e-9e72-1475cb0ed398"
            },
            "gsisId": "00-0030290",
            "number": "83",
            "srusId": "d48f6897-5e58-4233-b199-feeeb8600613",
            "position": "WR",
            "headshotKey": "bb9a7c7d-3bae-542e-b3bc-0a262650bb00",
            "enhancedPosition": "receiver"
          }
        ],
        "absoluteYardline": 70,
        "enhancedPlayType": "pass"
      },
      "dataSources": [
        {
          "uri": "https://s3-us-west-2.amazonaws.com/sr-ngs-play-breakdown-prod/57166/4018_assests.json",
          "name": "play-breakdown"
        }
      ]
    },
    ...
  ]
}
```

#### Paging
Use these parameters to page the results.

`/registry?limit=[number]&nextIndex=[from last result]`

- limit: The number of results you want returned with each call
- nextIndex: From the initial response, there will be a nextIndex used to get the next page

### GET /registry/[registry id]
Returns a single item by registry id.

Example:

https://6h4vl13py3.execute-api.us-west-2.amazonaws.com/prod/registry/ebdc23a0-73e4-11e7-af3f-cf2bde4ab38a

```json
{
  "id": "ebdc23a0-73e4-11e7-af3f-cf2bde4ab38a",
  "type": "play-breakdown",
  "initializationData": {
    "yfd": 10,
    "away": {
      "name": "Steelers",
      "alias": "PIT",
      "gsisId": "5000",
      "market": "Pittsburgh",
      "points": 17,
      "srusId": "cb2f9f1f-ac67-424e-9e72-1475cb0ed398"
    },
    "down": 1,
    "home": {
      "name": "Patriots",
      "alias": "NE",
      "gsisId": "4994",
      "market": "New England",
      "points": 36,
      "srusId": "97354895-8c77-4fd4-a860-32e62ea7382a"
    },
    "clock": "3:42",
    "title": "Touchdown! B. Roethlisberger 30 yd pass to C. Hamilton",
    "period": 4,
    "playType": "pass",
    "yardLine": 30,
    "playClock": 11,
    "awayPoints": 15,
    "homePoints": 36,
    "periodType": "quarter",
    "possession": "5000",
    "description": "(3:42) (Shotgun) 7-B.Roethlisberger pass deep right to 83-C.Hamilton for 30 yards, TOUCHDOWN.",
    "featurePlayers": [
      {
        "name": "Ben Roethlisberger",
        "team": {
          "gsisId": "5000",
          "srusId": "cb2f9f1f-ac67-424e-9e72-1475cb0ed398"
        },
        "gsisId": "00-0022924",
        "number": "07",
        "srusId": "ea357add-1a41-4a8b-8f34-bbfade7f4d98",
        "position": "QB",
        "headshotKey": "89080494-5e5d-5ccd-bf4a-699be236d124",
        "enhancedPosition": "passer"
      },
      {
        "name": "Cobi Hamilton",
        "team": {
          "gsisId": "5000",
          "srusId": "cb2f9f1f-ac67-424e-9e72-1475cb0ed398"
        },
        "gsisId": "00-0030290",
        "number": "83",
        "srusId": "d48f6897-5e58-4233-b199-feeeb8600613",
        "position": "WR",
        "headshotKey": "bb9a7c7d-3bae-542e-b3bc-0a262650bb00",
        "enhancedPosition": "receiver"
      }
    ],
    "absoluteYardline": 70,
    "enhancedPlayType": "pass"
  },
  "dataSources": [
    {
      "uri": "https://s3-us-west-2.amazonaws.com/sr-ngs-play-breakdown-prod/57166/4018_assests.json",
      "name": "play-breakdown"
    }
  ],
  "time": "2017-01-23T02:32:15.000Z",
  "timeMs": "1485138735000",
  "createdAt": "2017-07-28T22:34:30.506Z",
  "updatedAt": "2017-07-28T22:34:30.506Z",
  "filters": [
    {
      "id": "ebe01b40-73e4-11e7-af3f-cf2bde4ab38a",
      "itemId": "ebdc23a0-73e4-11e7-af3f-cf2bde4ab38a",
      "name": "teamId",
      "value": "4994",
      "createdAt": "2017-07-28T22:34:30.519Z",
      "updatedAt": "2017-07-28T22:34:30.519Z"
    },
    {
      "id": "ebe01b41-73e4-11e7-af3f-cf2bde4ab38a",
      "itemId": "ebdc23a0-73e4-11e7-af3f-cf2bde4ab38a",
      "name": "teamId",
      "value": "5000",
      "createdAt": "2017-07-28T22:34:30.519Z",
      "updatedAt": "2017-07-28T22:34:30.519Z"
    },
    {
      "id": "ebe01b42-73e4-11e7-af3f-cf2bde4ab38a",
      "itemId": "ebdc23a0-73e4-11e7-af3f-cf2bde4ab38a",
      "name": "classification",
      "value": "BIG_PLAY",
      "createdAt": "2017-07-28T22:34:30.519Z",
      "updatedAt": "2017-07-28T22:34:30.519Z"
    }
  ]
}
```

## Schedule API
Sportradar provides an API to access schedule information for games available in GameStream.


### /active-schedule
https://ptr2eaarra.execute-api.us-west-2.amazonaws.com/prod/active-schedule
Returns an object which includes an array of schedule weeks and a hash of games for each week.
```
{
"weeks": [{
  "id": "",    // srWeekId, this will be used as the id in games hash (can also be used as a filter in the registry)
  "type": "",  // season type pre, reg, pst
  "sKey": 0,   // key that can be used for sorting (helpful when response includes week for multiple season types)
  "number": 0, // season week number
  }],
  games: { //games is hash of games, keyed by srWeekId
    "srWeekId":[{
      "id": "", //srGameId (can also be used as a filter in the registry)
      "home":{
        "id": "", //team id for the home team
        "name": "", //home team name
        "alias": "", //home team alias
        "game_number": 0 //game number for the home team
      },
      "away":{
        "id": "", //team id for the away team
        "name": "", //away team name
        "alias": "", //away team alias
        "game_number": 0 //game number for the away team
      }
    }]
  }
}
```

Example:
https://ptr2eaarra.execute-api.us-west-2.amazonaws.com/prod/active-schedule

```json

{
  "weeks": [{
    "id": "4b642672-a0df-4747-90e9-c25c8e443d15",
    "type": "pre",
    "sKey": 0,
    "number": 0
  }, {
    "id": "d6ca5b5b-1dda-4212-9f41-74c4d2357b05",
    "type": "pre",
    "sKey": 1,
    "number": 1
  }],
  "games": {
    "4b642672-a0df-4747-90e9-c25c8e443d15": [{
      "id": "c18dc388-cb55-4c6b-b247-ef6c14c771f4",
      "home": {
        "id": "e627eec7-bbae-4fa4-8e73-8e1d6bc5c060",
        "name": "Dallas Cowboys",
        "alias": "DAL",
        "game_number": 1
      },
      "away": {
        "id": "de760528-1dc0-416a-a978-b510d20692ff",
        "name": "Arizona Cardinals",
        "alias": "ARI",
        "game_number": 1
      }
    }],
    "d6ca5b5b-1dda-4212-9f41-74c4d2357b05": [{
      "id": "11521ad9-fa31-4dd7-aa0e-6dd855db26f8",
      "home": {
        "id": "d5a2eb42-8065-4174-ab79-0a6fa820e35e",
        "name": "Cleveland Browns",
        "alias": "CLE",
        "game_number": 1
      },
      "away": {
        "id": "0d855753-ea21-4953-89f9-0e20aff9eb73",
        "name": "New Orleans Saints",
        "alias": "NO",
        "game_number": 1
      }
    }, {
      "id": "15b4c33c-b4f3-4f34-8e33-4a61f3c306b4",
      "home": {
        "id": "7b112545-38e6-483c-a55c-96cf6ee49cb8",
        "name": "Chicago Bears",
        "alias": "CHI",
        "game_number": 1
      },
      "away": {
        "id": "ce92bd47-93d5-4fe9-ada4-0fc681e6caa0",
        "name": "Denver Broncos",
        "alias": "DEN",
        "game_number": 1
      }
    }, {
      "id": "1f244edd-cd81-4160-9fab-4dc8f9326f5a",
      "home": {
        "id": "04aa1c9d-66da-489d-b16a-1dee3f2eec4d",
        "name": "New York Giants",
        "alias": "NYG",
        "game_number": 1
      },
      "away": {
        "id": "cb2f9f1f-ac67-424e-9e72-1475cb0ed398",
        "name": "Pittsburgh Steelers",
        "alias": "PIT",
        "game_number": 1
      }
    }]
  }
}

```
