# Splunk Search Processing Language (SPL) Queries

## View Linux Authentication Logs

```spl
source="/var/log/auth.log"
```

## Detect Failed SSH Login Attempts

```spl
source="/var/log/auth.log" "Failed password"
```

## Visualize Failed Login Attempts

```spl
source="/var/log/auth.log" "Failed password"
| timechart count
```
