# Create the structure
```
h5m add folder qvss_throughput
h5m add jq to qvss_throughput throughput '.results."quarkus3-jvm".load.avThroughput'
h5m add jq to qvss_throughput majorMinor '.config.QUARKUS_VERSION | split(".") | .[0:2] | join(".")'
h5m add jq to qvss_throughput startTime '.timing.start'
h5m add relativedifference rdNode to qvss_throughput range throughput domain startTime fingerprint majorMinor window 1 minPrevious 3 threshold 0.2 
```

# Upload data
```
# Baseline & Stable (Version 3.7.1)
echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 29482}}}, "config": {"QUARKUS_VERSION": "3.7.1"}, "timing": {"start": "2024-02-02T10:00:00Z"}}' > 26594.json
h5m upload 26594.json to qvss_throughput

echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 29715}}}, "config": {"QUARKUS_VERSION": "3.7.1"}, "timing": {"start": "2024-02-02T11:00:00Z"}}' > 26598.json
h5m upload 26598.json to qvss_throughput

echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 29561}}}, "config": {"QUARKUS_VERSION": "3.7.1"}, "timing": {"start": "2024-02-02T12:00:00Z"}}' > 26599.json
h5m upload 26599.json to qvss_throughput

echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 29583}}}, "config": {"QUARKUS_VERSION": "3.7.1"}, "timing": {"start": "2024-02-07T10:00:00Z"}}' > 26776.json
h5m upload 26776.json to qvss_throughput

# Big Regression ~70% drop (Version 3.7.3)
echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 8778}}}, "config": {"QUARKUS_VERSION": "3.7.3"}, "timing": {"start": "2024-02-19T10:00:00Z"}}' > 27271.json
h5m upload 27271.json to qvss_throughput

echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 9223}}}, "config": {"QUARKUS_VERSION": "3.7.3"}, "timing": {"start": "2024-02-19T11:00:00Z"}}' > 27272.json
h5m upload 27272.json to qvss_throughput

# Recovery (Version 3.7.3)
echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 29490}}}, "config": {"QUARKUS_VERSION": "3.7.3"}, "timing": {"start": "2024-02-19T12:00:00Z"}}' > 27279.json
h5m upload 27279.json to qvss_throughput

# Severe Regression ~93% drop (Version 3.7.4)
echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 2203}}}, "config": {"QUARKUS_VERSION": "3.7.4"}, "timing": {"start": "2024-02-22T10:00:00Z"}}' > 27405.json
h5m upload 27405.json to qvss_throughput

echo '{"results": {"quarkus3-jvm": {"load": {"avThroughput": 2206}}}, "config": {"QUARKUS_VERSION": "3.7.4"}, "timing": {"start": "2024-02-22T11:00:00Z"}}' > 27406.json
h5m upload 27406.json to qvss_throughput
```

# Find regression
```
h5m list value from qvss_throughput
```
^
|
```
┌─────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬─────────┐
│ id  │                                                        data                                                        │ node.id │
├─────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────┤
│  52 │ 29482                                                                                                              │     301 │
│  53 │ 2024-02-02T10:00:00Z                                                                                               │     401 │
│  54 │ 3.7                                                                                                                │     351 │
│  55 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 102 │ 29715                                                                                                              │     301 │
│ 103 │ 2024-02-02T11:00:00Z                                                                                               │     401 │
│ 104 │ 3.7                                                                                                                │     351 │
│ 105 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 152 │ 29561                                                                                                              │     301 │
│ 153 │ 2024-02-02T12:00:00Z                                                                                               │     401 │
│ 154 │ 3.7                                                                                                                │     351 │
│ 155 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 202 │ 29583                                                                                                              │     301 │
│ 203 │ 2024-02-07T10:00:00Z                                                                                               │     401 │
│ 204 │ 3.7                                                                                                                │     351 │
│ 205 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 252 │ 8778                                                                                                               │     301 │
│ 253 │ 2024-02-19T10:00:00Z                                                                                               │     401 │
│ 254 │ 3.7                                                                                                                │     351 │
│ 255 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 256 │ {"previous":29715.0,"last":29715.0,"value":8778.0,"ratio":-70.36428499082817,"domainvalue":"2024-02-19T10:00:00Z"} │     452 │
│ 302 │ 9223                                                                                                               │     301 │
│ 303 │ 2024-02-19T11:00:00Z                                                                                               │     401 │
│ 304 │ 3.7                                                                                                                │     351 │
│ 305 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 306 │ {"previous":29715.0,"last":29715.0,"value":8778.0,"ratio":-70.36428499082817,"domainvalue":"2024-02-19T10:00:00Z"} │     452 │
│ 352 │ 29490                                                                                                              │     301 │
│ 353 │ 2024-02-19T12:00:00Z                                                                                               │     401 │
│ 354 │ 3.7                                                                                                                │     351 │
│ 355 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 356 │ {"previous":29715.0,"last":29715.0,"value":8778.0,"ratio":-70.36428499082817,"domainvalue":"2024-02-19T10:00:00Z"} │     452 │
│ 402 │ 2203                                                                                                               │     301 │
│ 403 │ 2024-02-22T10:00:00Z                                                                                               │     401 │
│ 404 │ 3.7                                                                                                                │     351 │
│ 405 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 406 │ {"previous":29715.0,"last":29715.0,"value":8778.0,"ratio":-70.36428499082817,"domainvalue":"2024-02-19T10:00:00Z"} │     452 │
│ 452 │ 2206                                                                                                               │     301 │
│ 453 │ 2024-02-22T11:00:00Z                                                                                               │     401 │
│ 454 │ 3.7                                                                                                                │     351 │
│ 455 │ {"majorMinor":"3.7"}                                                                                               │     451 │
│ 456 │ {"previous":29561.0,"last":29561.0,"value":9223.0,"ratio":-59.26356703277289,"domainvalue":"2024-02-19T11:00:00Z"} │     452 │
└─────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴─────────┘
```
