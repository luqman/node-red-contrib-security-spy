# node-red-security-spy

## Security Spy Events
Simple Node-Red Subflow focused on SecuritySpy (NVR) event stream.

Note: No easy way to get camera number, use debug node to get camera number.

## Subflow Environment Variables

```javascript
PROTOCOL: http:
HOSTNAME: localhost
PORT: 8000
USERNAME:
PASSWORD:
AUTO_CONNECT: true
RETRY_DURATION: 30
LOG_EVENTS: false
```


If `AUTO_CONNECT` is `false` you have to manually inject to trigger connection.

## Output

Output 1: `connecting`, `connected` or `disconnected`

Output 2: Stream of parsed events from https://bensoftware.com/securityspy/web-server-spec.html#miscDiv

All events have;

```JSON
{"time":"20240928132817","number":"0" ...}
```

Personally I've not used these values for anything. `number` resets after re-connection.

Heartbeat

```JSON
{"camera":"X","event":"NULL","info":""}
```

If `camera` is `X` it means heartbeat/ping/keepalive, I use this to ping uptime checker.

File Written

```JSON
{"camera":"4","event":"FILE","info":"/Volumes/Storage/Security Spy/CameraX/{date}.mov"}
```

Motion

```JSON
{"camera":"4","event":"MOTION","info":{"x":"0","y":"1538","w":"420","h":"594"}}
```

Classify
```JSON
{"camera":"4","event":"CLASSIFY","info":{"HUMAN":100,"VEHICLE":0,"ANIMAL":1}}
```

Motion End
```JSON
{"time":"20240928132616","number":"22","camera":"4","event":"MOTION_END","info":""}
```

Trigger
```JSON
{"camera":"4","event":"TRIGGER_A","info":"128"}
```

All events can be found here https://bensoftware.com/securityspy/web-server-spec.html#miscDiv

> Personally I use `TRIGGER_A` in combimation with `MOTION_END` to create a sensor in Home Assistant.
