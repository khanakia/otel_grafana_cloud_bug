# OpenTelemetry Grafana Cloud Integration

This project demonstrates how to send OpenTelemetry traces to Grafana Cloud using the OTLP HTTP exporter.

## Issue Description

The current implementation has a URL encoding issue that causes the following error:

```
traces export: parse "https://otlp-gateway-prod-eu-west-2.grafana.net%2Fotlp/v1/traces": invalid URL escape "%2F"
```

<img width="958" height="109" alt="Screenshot 2025-08-12 at 5 23 37 PM" src="https://github.com/user-attachments/assets/ab269412-5c9f-42e4-a733-d2306e2ab13e" />


### Root Cause

The issue is in the `main.go` file where the endpoint URL is incorrectly formatted:

```go
otlptracehttp.WithEndpoint("otlp-gateway-prod-eu-west-2.grafana.net/otlp"),
```

The problem occurs because:
1. The URL is missing the `https://` protocol
2. The path `/otlp` is being URL-encoded to `%2Fotlp` incorrectly
3. The OTLP HTTP exporter expects a complete URL with protocol



### Running the Application

```bash
go mod vendor
go run .
```

The application will:
- Initialize OpenTelemetry SDK
- Create a test trace with an event
- Send the trace to Grafana Cloud
- Wait 30 seconds before shutting down
