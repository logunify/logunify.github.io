# iOS

> Note: Make sure you finish the [Setup](/examples) steps.

## Dependency

Add the following cocoapods dependency into your `Podfile`

```
pod 'LogUnify', '~> 0.1.5'
```

and import the library `import LogUnify`

## Integration

### Initialize Logger

You need to initialize LogUnify logger before using it. The `LogUnifyConfig` struct is responsible for holding the configuration details for logging events to a cloud endpoint. The struct is defined as below

```swift
public struct LogUnifyConfig {
    // TODO: set a default value for this, use the cloud solution endpoint.
    var serverUrl: String;
    var apiKey: String;
    var batchSize: Int = 10;
    var timerTrigger: Int = 60;

    public init(serverUrl: String,
                apiKey: String,
                batchSize: Int = 10,
                timerTrigger: Int = 60) {
        self.serverUrl = serverUrl
        self.apiKey = apiKey
        self.batchSize = batchSize
        self.timerTrigger = timerTrigger
    }
}
```

It has the following properties

- serverUrl: A string representing the endpoint of the cloud solution to which the logs will be sent. This property is required.
- apiKey: A string representing the API key required for authentication with the cloud solution endpoint. This key should match the backend service apiKey, otherwise the logging requests will get unauthorized errors.
- batchSize: An integer representing the number of events that will be batched together before being sent to the cloud solution endpoint. The default value is set to 10.
- timerTrigger: An integer representing the interval, in seconds, at which the batch of events will be sent to the cloud solution endpoint if there are less than `batchSize` logs. The default value is set to 60 seconds.

Then initialize the logger by

```swift
let config = LogUnifyConfig("http://localhost:8081/api/events/_bulk", timerTrigger: 10)
try! UnifyLogger.initialize(config)
```

The logs will be cached on the user's device, until either of the following conditions is met:

1. The number of logs cached on the user's device is greater than or equal to `batchSize`.
2. The time elapsed since the last attemp to send the logs to the cloud service is greater than or equal to `timerTrigger` seconds.

With either of those conditions is met, the cached logs will be sent to the cloud service.

### Log Event

The event logging API is very similar to the protobuf API. Initialize an event variable, fill the fields, then call `.log()` function to log the event.

```swift

var event = UserActivity.with {
    $0.userID = "iOS App"
    $0.event = Event.click
    $0.buttonType = ButtonType.next
    $0.sessionID = "iOS Session"
    $0.surface = Surface.screen1
    $0.stringArray = ["a", "b", "c"]
    $0.intArray = [1, 2, 3]
    $0.stringIntMap["key"] = 123
}
```

then call

```swift
event.log()
```

### TODO: Is there any one liner format for swift?

and you should see your log in your bigquery table in a few seconds / minutes, depending on your `timerTrigger` config.

You can find an example app in this [repository](https://github.com/logunify/examples/tree/main/ios)
