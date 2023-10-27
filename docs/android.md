# Android

> Note: Make sure you finish the [Setup](/examples) steps.

## Dependency

Add the following dependencies into your code `build.gradle` file.

```
implementation 'com.logunify:android-sdk:0.1.2'
implementation "com.google.protobuf:protobuf-java:3.21.12"
```

## Integration

### Initialize Logger

You need to initialize LogUnify logger before using it. There are two main properties of the logger, the logger `apiKey` and the logger receive URL. The configuration is read directly from the `AndroidManifest.xml` file, you can config like below:

```xml
<meta-data
        android:name="LogunifyAPIKey"
        android:value="api_key_1" />
<meta-data
        android:name="LogunifyReceiverUrl"
        android:value="http://10.0.2.2:8081/api/events/_bulk" />
```

then initialize the logger in your Android app entry point:

```java
Logger.init(getApplicationContext());
```

### Log Events

The event logging API is pretty straightforward. We use builder pattern like below:

```java
UserActivitySchema.UserActivity.newBuilder()
        .setEvent(UserActivitySchema.Event.CLICK)
        .setButtonType(UserActivitySchema.ButtonType.FAB)
        .setSurface(UserActivitySchema.Surface.BOTTOM_FAB)
        .setSessionId(SessionManager.getSessionID())
        .setUserId("user_id_1")
        .log();
```

and you should see the log in your data warehouse in a few minutes if not seconds.

You can find an example app in this [repository](https://github.com/logunify/examples/tree/main/android).
