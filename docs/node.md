# Node

> Note: Make sure you finish the [Setup](/examples) steps.

## Dependency

```json
  "@logunify/node-sdk": "^0.1.2",
```

and import the library `import LogUnifyLogger from "@logunify/node-sdk"`.

## Integration

### Initialize Logger

You need to initialize the LogUnify logger before using it. The initialization `Options` struct is responsible for holding the configuration details. The struct is defined as below

```typescript
export type Options = {
  apiKey: string;
  receiverURL?: string,
  batchInterval?: number;
  minBatchSize?: number;
  enableDebugLog?: boolean;
};
```

It has the following properties

- `apiKey`: A string representing the API key required for authentication with the cloud solution endpoint. This key should match the backend service apiKey, otherwise the logging requests will get unauthorized errors.
- `receiverURL`: an optional string that specifies the full url of the logging server to receive events. Default value is `http://localhost:8081/api/events/_bulk`.
- `batchInterval`: an optional number that represents the time interval in milliseconds between two logging events. A value of zero means to disable batching. Default value is `5000`.
- `minBatchSize`: an optional number that specifies the minimum number of events needed to trigger a logging upload to the server. Default value is `10`.
- `enableDebugLog`: an optional boolean that enables debugging logs to the command-line interface (CLI).

Initialize the logger

```typescript
LogUnifyLogger.setup({
  apiKey: "api_key_1"
});
```

The logs will be cached on the client, unither either of the following conditions is met:

1. The number of logs cached on the device is greater than or equal to minBatchSize.
2. The time elapsed since the last logging attemp is greater than or equal to the `batchInternal` threshold.

### Log Event

To log an event, you should import the generated typescript SDK from LogUnify cli tool, and call

```typescript
UserActivity.fromObject({
  user_id: userId,
  surface: Surface.SCREEN_1,
  button_type: ButtonType.NEXT,
  event: Event.IMPRESSION,
  session_id: sessionId,
  string_array: ["str1", "str2"],
  int_array: [1, 2],
  string_int_map: {
    str1: 1,
    str2: 2,
  },
}).log();
```

You should be able to see the data in your data warehouse in a few seconds or minutes, depending on the logging configuration.

You can find an example app in this [repository](https://github.com/logunify/examples/tree/main/type_script).
