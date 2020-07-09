# OT Advanced Insights Documentation
## Conpects
***Connection*** \
A new connection is created each time whenever a user entering a opentok session, and build connection to the server.\
\
***Publisher*** \
If a user decide to publish his video/audio, a new publisher will created under his opentok connection. Usually, one connection has one publisher, however you got two publisher if u share your screen since its two different stream. \
\
***Subscriber*** \
When a user build opentok connection with opentok server, several subscribers will be created. Ideally, if there are n users besides the user himself publishing their stream, there will be n subscribers.

---
## Data Model
***Relationship***
* TandemSession - TandemOTConnection: one to many
* TandemOTConnection - TandemOTConnectionPublisher: one to many
* TandemOTConnection - TandemOTConnectionSubscriber: one to many

***TandemOTConnection Definition***
```javascript
module.exports = {

  primaryKey: 'id',

  attributes: {
    tandem_session: {
      model: 'TandemSession',
      required: true
    },
    ot_session_id: {
      type: 'string',
      required: true
    },
    connection_id: {
      type: 'string'
    },
    media_mode: {
      type: 'string'
    },
    connection_created_at: {
      type: 'string'
    },
    connection_destroyed_at: {
      type: 'string'
    },
    sdk_type: {
      type: 'string'
    },
    sdk_version: {
      type: 'string'
    },
    browser: {
      type: 'string'
    },
    browser_version: {
      type: 'string'
    },
    guid: {
      type: 'string'
    },
    user_agent: {
      type: 'string'
    },
    publishers: {
      collection: 'tandemotconnectionpublisher',
      via: 'tandemotconnection'
    },
    subscribers: {
      collection: 'tandemotconnectionsubscriber',
      via: 'tandemotconnection'
    }
  }
};
```
***TandemOTConnectionPublisher Definition***
```javascript
module.exports = {

  primaryKey: 'id',

  attributes: {
    tandemotconnection: {
      model: 'tandemotconnection'
    },
    publisher_created_at: {
      type: 'string'
    },
    publisher_destroyed_at: {
      type: 'string'
    },
    stream_id: {
      type: 'string'
    },
    stream_stats_collection: {
      type: 'json',
      columnType: 'array'
    }
  }
};

```
***TandemOTConnectionSubscriber Definition***
```javascript
module.exports = {

  primaryKey: 'id',

  attributes: {
    tandemotconnection: {
      model: 'tandemotconnection'
    },
    subscriber_created_at: {
      type: 'string'
    },
    subscriber_destroyed_at: {
      type: 'string'
    },
    stream_id: {
      type: 'string'
    },
    stream_stats_collection: {
      type: 'json',
      columnType: 'array'
    }
  }
};
```
---
## API
```javascript
// When a session ends, opentok callback will call this function in TandemController.
injectAdvancedInsightsIntoDatabase(tandemId: string): TandemOTConnection[] {...};
```
---
### GraphQL
see opentok's graphql schema [here](https://insights.opentok.com/)
```
# example gql
  {
    project(projectId: ${tokProjectId}){
     sessionData {
      sessions(sessionIds: ["${otSessionId}"]) {
        resources {
          sessionId
          mediaMode
          publisherMinutes
          subscriberMinutes
          meetings {
            totalCount
            resources {
              meetingId
              publisherMinutes
              subscriberMinutes
              connections {
                totalCount
                resources {
                  connectionId
                  createdAt
                  destroyedAt
                  sdkType
                  sdkVersion
                  browser
                  browserVersion
                  guid
                  userAgent
                  publishers {
                    totalCount
                    resources {
                      createdAt
                      destroyedAt
                      connectionId
                      stream {
                        streamId
                      }
                      streamStatsCollection {
                        totalCount
                        resources {
                          createdAt
                          hasAudio
                          hasVideo
                          audioCodec
                          audioLatencyMs
                          audioBitrateKbps
                          audioPacketLoss
                          videoCodec
                          videoLatencyMs
                          videoBitrateKbps
                          videoPacketLoss
                          videoResolution
                        }
                      }
                    }
                  }
                  subscribers {
                    totalCount
                    resources {
                      subscriberId
                      connectionId
                      createdAt
                      destroyedAt
                      stream {
                        streamId
                      }
                      streamStatsCollection {
                        totalCount
                        resources {
                          createdAt
                          subscribeToAudio
                          subscribeToVideo
                          audioCodec
                          audioLatencyMs
                          audioBitrateKbps
                          audioPacketLoss
                          videoCodec
                          videoLatencyMs
                          videoBitrateKbps
                          videoPacketLoss
                          videoResolution
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }
     }
    }
  }
```
``` json
# example response
{
  "data": {
    "project": {
      "sessionData": {
        "sessions": {
          "resources": [
            {
              "sessionId": "2_MX40NjIwNDk3Mn5-MTU5NDI4NjA4Nzc3MX5rY0FSeUp3WHlxdDlkbUc1WXlJTHFQbDV-fg",
              "mediaMode": "routed",
              "publisherMinutes": 0.27841666666666665,
              "subscriberMinutes": 0,
              "meetings": {
                "totalCount": 1,
                "resources": [
                  {
                    "meetingId": "edb26699-9eba-4a99-a32f-6a18841080ce",
                    "publisherMinutes": 0.27841666666666665,
                    "subscriberMinutes": 0,
                    "connections": {
                      "totalCount": 1,
                      "resources": [
                        {
                          "connectionId": "24676616-dbc2-4cb7-a986-14dafd452ab0",
                          "createdAt": "2020-07-09T09:14:57.530Z",
                          "destroyedAt": "2020-07-09T09:15:14.485Z",
                          "sdkType": "js",
                          "sdkVersion": "2.17.0",
                          "browser": "Chrome",
                          "browserVersion": "83",
                          "guid": "0feee5f3-8dbc-4918-b693-6b7df0c5027c",
                          "userAgent": "Mozilla%2F5.0+%28Macintosh%3B+Intel+Mac+OS+X+10_14_6%29+AppleWebKit%2F537.36+%28KHTML%2C+like+Gecko%29+Chrome%2F83.0.4103.116+Safari%2F537.36",
                          "publishers": {
                            "totalCount": 1,
                            "resources": [
                              {
                                "createdAt": "2020-07-09T09:14:57.780Z",
                                "destroyedAt": "2020-07-09T09:15:14.485Z",
                                "connectionId": "24676616-dbc2-4cb7-a986-14dafd452ab0",
                                "stream": {
                                  "streamId": "7bc2656b-cbc9-442d-a1e0-4af9aa2e864a"
                                },
                                "streamStatsCollection": {
                                  "totalCount": 1,
                                  "resources": [
                                    {
                                      "createdAt": "2020-07-09T09:15:03.500Z",
                                      "hasAudio": true,
                                      "hasVideo": true,
                                      "audioCodec": "opus",
                                      "audioLatencyMs": null,
                                      "audioBitrateKbps": 1.88,
                                      "audioPacketLoss": 0,
                                      "videoCodec": "VP8",
                                      "videoLatencyMs": null,
                                      "videoBitrateKbps": 8.25,
                                      "videoPacketLoss": 0,
                                      "videoResolution": "1280x720"
                                    }
                                  ]
                                }
                              }
                            ]
                          },
                          "subscribers": {
                            "totalCount": 0,
                            "resources": []
                          }
                        }
                      ]
                    }
                  }
                ]
              }
            }
          ]
        }
      }
    }
  }
}
```
