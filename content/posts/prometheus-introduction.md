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

## Core Features

### Data Model

The data model in Prometheus is based on time-series data, where each series is identified by a unique metric name and a set of key-value pairs called labels.

Each time-series is composed of a sequence of data points, where each data point consists of a timestamp and a numeric value.

Metrics are organized in a hierarchical namespace, with each level separated by a forward-slash (/) character. For example, the metric "http_requests_total" would represent the total number of HTTP requests, while "http_request_duration_seconds" could represent the duration of each individual request.

Labels are used to add additional dimensions to a metric and to group metrics together. Labels are key-value pairs, where the key is a string and the value is also a string. For example, a metric for HTTP request duration could have a label for the status code, such as "200" or "404", and another label for the endpoint, such as "/users" or "/admin". This allows for easy filtering, aggregation and querying of metrics.

Prometheus supports a variety of data types, including counters, gauges, histograms, and summaries. Counters are monotonically increasing values that are typically used to track the rate of events, such as the number of requests per second. Gauges are instantaneous values that can increase or decrease, such as the current temperature. Histograms and summaries are used to track the distribution of values, such as the request latencies.

Prometheus data model is based on time-series data, where each series is identified by a unique metric name, and a set of key-value pairs called labels. Prometheus supports various data types, which allow to track the rate of events, instantaneous values, and distribution of values.

[More Info](https://prometheus.io/docs/concepts/data_model/#data-model)


### Metrics Transfer Format

Prometheus uses its own data format to transfer metrics between Prometheus components and exporters. Is a text-based format that is designed to be human-readable and easy to parse.

Uses a line-based structure, where each line represents a single metric. Each metric is defined by a metric name, followed by a set of key-value pairs that define the labels of the metric. The metric value is then appended to the end of the line.

The metric name and labels are separated by a space, while the key-value pairs are separated by an equals sign (=). Multiple key-value pairs are separated by a comma.

Here's an example of a metric:

```json
http_request_duration_seconds{method="GET", endpoint="/users"} 0.123
```
In this example, the metric name is "http_request_duration_seconds", and it has two labels "method" and "endpoint" with the respective values "GET" and "/users". The value of the metric is 0.123.

The format also supports sending multiple samples within the same metric, allowing for more efficient transfer of metrics. The format also includes a way to encode "help" and "type" of the metric, that is human-readable information about the metric and it's type.

[More Info](https://training.promlabs.com/training/introduction-to-prometheus/prometheus-an-overview/metrics-transfer-format)

### Query Language

__Prometheus Query Language (PromQL)__ is a powerful query language that is used to process and analyze metrics stored in Prometheus. It is designed to be expressive, yet easy to use, and allows users to perform complex queries and calculations on the collected metrics.

PromQL supports a variety of operations, including:

- __Aggregation__: PromQL allows for aggregation of metrics based on their labels, for example, calculating the average request duration for all requests with a specific endpoint.

- __Arithmetic__: PromQL supports basic mathematical operations such as addition, subtraction, multiplication, and division.

- __Filtering__: PromQL allows for filtering metrics based on their labels, for example, selecting only the metrics for requests with a specific status code.

- __Vector matching__: PromQL allows for selecting metrics based on their labels, for example, selecting all metrics that match a specific label value.

- __Functions__: PromQL includes a variety of built-in functions, such as rate() to calculate the rate of change of a metric over time, histogram_quantile() to calculate quantiles of a histogram, and time() to select the timestamp of a metric.

PromQL also supports using subqueries to perform more complex calculations. For example, calculating the average request duration for all requests with a specific endpoint, then calculating the rate of change of that average over time.

PromQL also supports the use of regular expressions for matching metric names and label values, and in-built functions for working with time series.

Cheat Sheet: https://promlabs.com/promql-cheat-sheet/

[More Info](https://prometheus.io/docs/prometheus/latest/querying/basics/)

### Alerting

Prometheus includes a built-in alerting system that allows users to define rules that trigger alerts based on the results of PromQL queries. The alerting system is composed of two main components: __Prometheus server__ and __Alertmanager__.

- __Prometheus Server__: Prometheus server evaluates the alerting rules at a specified interval, and if a rule's condition is met, it sends an alert to the Alertmanager. The alert includes the metric name, labels, and value that triggered the alert, as well as the time that the alert was triggered.

- __Alertmanager__: The Alertmanager is responsible for sending notifications about detected issues. It can be configured to route alerts to different receivers based on their severity and other criteria. It can also handle alert deduplication, aggregation, and silence management.

Alertmanager can send notifications via various methods like email, slack, pagerduty, etc. Alerts can also be grouped together, so that multiple alerts can be sent as a single notification, and the alertmanager can also be configured to send notifications to different people or teams depending on the alert's severity or the time of day.

Users can define alerts in Prometheus configuration file, using PromQL queries to specify the conditions that should trigger the alert. Users can also specify the severity level of the alerts, and configure how often Prometheus should check for alerting conditions.






