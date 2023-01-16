+++
title = "Prometheus 101"
date = "2023-01-16T10:17:17-05:00"
author = "Daniel Puig"
authorTwitter = "" #do not include @
cover = "https://upload.wikimedia.org/wikipedia/commons/3/38/Prometheus_software_logo.svg"
tags = ["monitoring", "observability", "metrics","prometheus"]
keywords = ["", ""]
description = "The open-source monitoring solution"
showFullContent = false
readingTime = false
hideComments = false
color = "orange" #color from the theme settings
+++


Prometheus is an open-source systems monitoring and alerting toolkit. It was originally developed at SoundCloud in 2012. Prometheus is designed for monitoring and alerting of systems and applications in dynamic and distributed environments. It can collect metrics from various sources, including from application code, and provides a powerful query language for processing and analyzing the collected data. Prometheus also includes a built-in alert manager for sending notifications about detected issues. It is a popular choice for monitoring in cloud-native environments and is often used in conjunction with Kubernetes.

### System

Prometheus is composed of a few key components:

- __Prometheus Server__: This is the core component that collects metrics from various sources and stores them in a time-series database. It also provides a query language for processing and analyzing the collected data.

- __Exporters__: These are special-purpose exporters that collect metrics from specific systems or applications and make them available to Prometheus. Examples include exporters for Linux system metrics, MySQL, and HAProxy.

- __Alertmanager__: This component is responsible for sending notifications about detected issues. It can be configured to route alerts to different receivers based on their severity and other criteria.

- __Pushgateway__: This component allows short-lived jobs to push metrics to Prometheus. It is commonly used in situations where it is not possible to run the Prometheus server as a long-running process.

- __Grafana__: This is a popular open-source visualization tool that can be used in conjunction with Prometheus to display metrics in a more user-friendly way.

![Architecture](https://prometheus.io/assets/architecture.png)

### Core Features

__Data Model__ : The data model in Prometheus is based on time-series data, where each series is identified by a unique metric name and a set of key-value pairs called labels.

Each time-series is composed of a sequence of data points, where each data point consists of a timestamp and a numeric value.

Metrics are organized in a hierarchical namespace, with each level separated by a forward-slash (/) character. For example, the metric "http_requests_total" would represent the total number of HTTP requests, while "http_request_duration_seconds" could represent the duration of each individual request.

Labels are used to add additional dimensions to a metric and to group metrics together. Labels are key-value pairs, where the key is a string and the value is also a string. For example, a metric for HTTP request duration could have a label for the status code, such as "200" or "404", and another label for the endpoint, such as "/users" or "/admin". This allows for easy filtering, aggregation and querying of metrics.

Prometheus supports a variety of data types, including counters, gauges, histograms, and summaries. Counters are monotonically increasing values that are typically used to track the rate of events, such as the number of requests per second. Gauges are instantaneous values that can increase or decrease, such as the current temperature. Histograms and summaries are used to track the distribution of values, such as the request latencies.

Prometheus data model is based on time-series data, where each series is identified by a unique metric name, and a set of key-value pairs called labels. Prometheus supports various data types, which allow to track the rate of events, instantaneous values, and distribution of values.

[More Info](https://prometheus.io/docs/concepts/data_model/#data-model)


__Metrics Transfer Format__ : Prometheus uses its own data format to transfer metrics between Prometheus components and exporters. Is a text-based format that is designed to be human-readable and easy to parse.

Uses a line-based structure, where each line represents a single metric. Each metric is defined by a metric name, followed by a set of key-value pairs that define the labels of the metric. The metric value is then appended to the end of the line.

The metric name and labels are separated by a space, while the key-value pairs are separated by an equals sign (=). Multiple key-value pairs are separated by a comma.

Here's an example of a metric:

```json
http_request_duration_seconds{method="GET", endpoint="/users"} 0.123
```
In this example, the metric name is "http_request_duration_seconds", and it has two labels "method" and "endpoint" with the respective values "GET" and "/users". The value of the metric is 0.123.

The format also supports sending multiple samples within the same metric, allowing for more efficient transfer of metrics. The format also includes a way to encode "help" and "type" of the metric, that is human-readable information about the metric and it's type.

[More Info](https://training.promlabs.com/training/introduction-to-prometheus/prometheus-an-overview/metrics-transfer-format)


