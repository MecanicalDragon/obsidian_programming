### To create a dashboard:
1. Create a dashboard in Grafana UI and save it in a dedicated directory.
2. Define a `datasource` variable of type `datasource` in dashboard variables. Pick a correct source type for it (Prometheus, Victoria).
3. List all label names with `label_names()` function. Use this knowledge in the following steps.
4. You need a label that uniquely determines your application, like `component`. Define a variable with it.
5. Define an `environment` variable of type `query` in dashboard variables. Scrap some metric that definitely exists - `system_cpu_count` for example, or `jvm_info` (Search for [label_values](https://grafana.com/docs/grafana/latest/datasources/prometheus/)). Also, you can use the function `up{}` - that's an analog of ping for prometheus.
	1. `label_values(environment)` - returns all existing environments.
	2. `label_values(system_cpu_count{component="$appname"}, environment)` or `label_values(up{component="$appname"}, environment)` - returns only environments that contain metrics with your component label.
6. Define `interval` variable of type `interval` to pick data for this interval in metrics.

### How to define metrics:
- `metric_name{tag=”tag”}[$interval]`
- `http_request_duration_seconds_bucket{environment="$environment", component="$appname", method="POST", uri="/some/uri", status="200"}[$interval]`
- `sum(metrics{}) by (tag)`
- `{{variable}}` – used in metric legend to expand all values. Also, enable the breaking metric by its value.
- `process_uptime_seconds{environment="$environment", component="$appname"} + on(pod) group_left(app_version) (0 * app_version{environment="$environment", component="$appname"})`

### Some guidelines:
- Buckets are not precise but good to aggregate data among several pods. Bucket data should be shown in a *heatmap*.
- Histogram quantiles are more precise but require extra computations on the application side, and cannot be aggregated.
- From the alerting point of view metrics of the specific pod are not so important: pods rise and die. We should lean on service metrics to fire alerts.

[Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)
[Query functions](https://prometheus.io/docs/prometheus/latest/querying/functions/#rate)
[Aggregation operators](https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators)
[Histograms and Summaries](https://prometheus.io/docs/practices/histograms/)
