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

In Prometheus, a Counter is a cumulative metric that represents a single numerical value that only ever increases. It is used to track the number of occurrences of a specific event or action, such as the number of requests served, the number of errors, or the number of items sold. Counters are useful for tracking metrics that are accumulated over time, such as the total number of requests served by a web server.

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
		w.Write([]byte("Hello, World!"))
	})

	// Expose the Prometheus metrics endpoint
	http.Handle("/metrics", promhttp.Handler())

	http.ListenAndServe(":8080", nil)
}
```

In this example, we are creating a counter called "myapp_requests_total" that keeps track of the total number of requests served by the application. The counter is registered with Prometheus and incremented for each request that the application handles. The Prometheus client library also provides a way to expose the metrics in an HTTP endpoint, which we can access on the path "/metrics" to scrape the metrics.

Note that it's important to not reset the counter as it's a cumulative metric, if you need to track a metric that can be resetted you should use Gauge instead.

## Gauge

A metric that represents a single numerical value that can arbitrarily go up and down, such as the temperature of a machine.

## Histogram 

A metric that samples observations and provides a statistical summary of the distribution of the values, such as request latencies.

## Summary 

A metric that also samples observations and provides a statistical summary, but it also calculates quantiles (e.g. median, 90th percentile) of the distribution, such as a summary of request durations.