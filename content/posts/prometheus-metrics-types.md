+++
title = "Prometheus Metrics Types"
date = "2023-01-24T22:35:11-05:00"
author = "Daniel Puig"
authorTwitter = "dpuiger" #do not include @
cover = "https://upload.wikimedia.org/wikipedia/commons/3/38/Prometheus_software_logo.svg"
tags = ["monitoring", "observability", "metrics","prometheus"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "orange" #color from the theme settings
draft = false
+++

Prometheus is a powerful open-source monitoring system that is widely used to collect, store and analyze time series data. One of the key features of Prometheus is its ability to collect a wide range of metrics, which can be used to monitor the performance of your infrastructure and applications. In this blog post, we will take a closer look at the different types of metrics that Prometheus supports, including counters, gauges, histograms, and summaries. Understanding the different types of metrics that Prometheus can collect is essential for making the most of this powerful monitoring tool. Whether you are a DevOps engineer, a system administrator, or a developer, this post will provide you with valuable insights into how to effectively monitor your systems and applications with Prometheus.

[More Info](https://prometheus.io/docs/concepts/metric_types/)

## Counter

A cumulative metric that represents a single numerical value that only ever increases. It is used to track the number of occurrences of a specific event or action, such as the number of requests served, the number of errors, or the number of items sold. Counters are useful for tracking metrics that are accumulated over time, such as the total number of requests served by a web server.

In Golang, you can use the Prometheus client library to expose a Counter metric. Here's an example of how to create and increment a Counter in Golang:

```go
package main

import (
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"net/http"
)

var (
	requestsCounter = prometheus.NewCounter(prometheus.CounterOpts{
		Name: "myapp_requests_total",
		Help: "The total number of requests served by the application",
	})
)

func main() {
	// Register the counter with Prometheus
	prometheus.MustRegister(requestsCounter)

	// Increment the counter for each request
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		requestsCounter.Inc()
		w.Write([]byte("Hello, Prometheus World Counter!"))
	})

	// Expose the Prometheus metrics endpoint
	http.Handle("/metrics", promhttp.Handler())

	http.ListenAndServe(":8080", nil)
}
```

In this example, we are creating a counter called "myapp_requests_total" that keeps track of the total number of requests served by the application. The counter is registered with Prometheus and incremented for each request that the application handles. The Prometheus client library also provides a way to expose the metrics in an HTTP endpoint, which we can access on the path "/metrics" to scrape the metrics.

Note that it's important to not reset the counter as it's a cumulative metric, if you need to track a metric that can be resetted you should use Gauge instead.

### Exposition

```sh
# HELP myapp_requests_total The total number of requests served by the application
# TYPE myapp_requests_total counter
myapp_requests_total 29
```

### Counter Rate of Increase

__rate ()__

__irate()__

__increase()__

Example:

```promql
rate(myapp_requests_total[2m])
```


## Gauge

A Gauge is a metric that represents a single numerical value that can arbitrarily go up and down. It is used to track a value that can change over time, such as the temperature of a machine, the number of active connections, or the available memory. Gauges are useful for tracking metrics that change dynamically, such as the number of users currently connected to a website.

In Golang, you can use the Prometheus client library to expose a Gauge metric. Here's an example of how to create and update a Gauge in Golang:

```go
package main

import (
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"net/http"
)

var (
	activeConnections = prometheus.NewGauge(prometheus.GaugeOpts{
		Name: "myapp_active_connections",
		Help: "The number of active connections to the application",
	})
)

func main() {
	// Register the gauge with Prometheus
	prometheus.MustRegister(activeConnections)

	// Update the gauge for each request
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		activeConnections.Add(1)
		defer activeConnections.Dec(1)
		w.Write([]byte("Hello, Prometheus World Gauge!"))
	})

	// Expose the Prometheus metrics endpoint
	http.Handle("/metrics", promhttp.Handler())

	http.ListenAndServe(":8080", nil)
}
```

We are creating a gauge called "myapp_active_connections" that keeps track of the number of active connections to the application. The gauge is registered with Prometheus and incremented by 1 for each request that the application handles. Then we decrement the gauge by 1 to reflect the end of the request. The Prometheus client library also provides a way to expose the metrics in an HTTP endpoint, also here we can access on the path "/metrics" to scrape the metrics.

Note that you can also use the Set(float64) method to set the value of a gauge, or the Inc() or Dec() method to increment or decrement the value by 1.

### Exposition

```sh
# HELP myapp_active_connections The number of active connections to the application.
# TYPE myapp_active_connections gauge
myapp_active_connections 34
```

## Histogram 

Is a metric that samples observations and provides a statistical summary of the distribution of the values. It is used to track the distribution of a metric over time, such as request latencies, response sizes, or durations of an operation. Histograms are useful for analyzing the performance of an application or service by providing a detailed view of how the metric is distributed across different values.

In Golang, you can use the Prometheus client library to expose a Histogram metric. Here's an example of how to create and observe a Histogram in Golang:

```go
package main

import (
	"time"
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"net/http"
)

var (
	responseTime = prometheus.NewHistogram(prometheus.HistogramOpts{
		Name: "myapp_response_time_seconds",
		Help: "The response time of the application in seconds",
		Buckets: prometheus.LinearBuckets(0.01, 0.1, 10),
	})
)

func main() {
	// Register the histogram with Prometheus
	prometheus.MustRegister(responseTime)

	// Observe the response time for each request
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		start := time.Now()
		w.Write([]byte("Hello, Prometheus World Histograms!"))
		responseTime.Observe(time.Since(start).Seconds())
	})

	// Expose the Prometheus metrics endpoint
	http.Handle("/metrics", promhttp.Handler())

	http.ListenAndServe(":8080", nil)
}
```

Here a histogram called "myapp_response_time_seconds" that keeps track of the response time of the application in seconds. We set up a set of linear buckets to observe the metric and we use the Observe(float64) method to record the response time of the request. The Prometheus client library also provides a way to expose the metrics in an HTTP endpoint, which we can access on the path "/metrics" to scrape the metrics.

Histograms in Prometheus use Buckets to aggregate observations, Buckets are predefined ranges of values that are used to count the observations that fall within each range. It's important to define the appropriate Buckets for your use case to get a meaningful Histogram.

### Exposition

```sh
# HELP myapp_response_time_seconds The response time of the application in seconds
# TYPE myapp_response_time_seconds histogram
myapp_response_time_seconds_bucket{le="0.01"} 0
myapp_response_time_seconds_bucket{le="0.1"} 0
myapp_response_time_seconds_bucket{le="10"} 0
myapp_response_time_seconds_sum 0
myapp_response_time_seconds_count 0
```

__Quantiles from Histograms__

```promql
histogram_quantile(<target_quantile>, <histogram>)
```

Example:

```promql
# 90th percentile latency,
# averaged over the last 5 minutes.

histogram_quantile(0.9, rate(myapp_response_time_seconds_bucket[5m]))
```

```promql
# 90th percentile latency for each path/method,
# combination, averaged over the last 5 minutes.

histogram_quantile(
    0.9,
    sum by(path, method, le) ( 
        rate(myapp_response_time_seconds_bucket[5m])
    )
)
```

__Average Latencies__

```promql
# Average response over the las 5 minutes.

rate(myapp_response_time_seconds_sum[5m]) 
/ 
rate (myapp_response_time_seconds_count[5m])
```

```promql
# Aggregated per path/method average response.

sum by(path, method) (rate(myapp_response_time_seconds_sum[5m]))
/ 
sum by(path, method) (rate (myapp_response_time_seconds_count[5m]))
```

## Summary 

A Summary is a metric that also samples observations and provides a statistical summary, but it also calculates quantiles (e.g. median, 90th percentile) of the distribution. It is used to track the distribution of a metric over time, such as request latencies, response sizes, or durations of an operation. Summaries are similar to Histograms, but they also provide quantile information, which can be useful for understanding the distribution of a metric more precisely.

In Golang, you can use the Prometheus client library to expose a Summary metric. Here's an example of how to create and observe a Summary in Golang:

```go
package main

import (
	"time"
	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"net/http"
)

var (
	responseTime = prometheus.NewSummary(prometheus.SummaryOpts{
		Name: "myapp_response_time_seconds",
		Help: "The response time of the application in seconds",
		Objectives: map[float64]float64{0.5: 0.05, 0.9: 0.01, 0.99: 0.001},
	})
)

func main() {
	// Register the summary with Prometheus
	prometheus.MustRegister(responseTime)

	// Observe the response time for each request
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		start := time.Now()
		w.Write([]byte("Hello, Prometheus World Summary!"))
		responseTime.Observe(time.Since(start).Seconds())
	})

	// Expose the Prometheus metrics endpoint
	http.Handle("/metrics", promhttp.Handler())

	http.ListenAndServe(":8080", nil)
}
```

In this example, we are creating a Summary called "myapp_response_time_seconds" that keeps track of the response time of the application in seconds. We use the Observe(float64) method to record the response time of the request and set objectives which are a map of quantiles to their absolute error. The Prometheus client library also provides a way to expose the metrics in an HTTP endpoint, which we can access on the path "/metrics" to scrape the metrics.

The quantiles are calculated based on the observations that have been recorded, and they are exposed as a separate metric in Prometheus. This allows you to easily query the median, 90th percentile, or other quantiles of the distribution directly, which can be useful for troubleshooting performance issues or identifying outliers.

### Exposition

```sh
# HELP myapp_response_time_seconds The response time of the application in seconds.
# TYPE myapp_response_time_seconds summary
myapp_response_time_seconds{quantile="0.5"} 0
myapp_response_time_seconds{quantile="0.9"} 0
myapp_response_time_seconds{quantile="0.99"} 0
myapp_response_time_seconds_sum 0
myapp_response_time_seconds_count 0
```

For now we will leave this documentation here for now, soon we will wave more on this topic with much more examples.