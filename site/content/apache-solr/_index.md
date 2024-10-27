---
title: apache-solr
---

## Overview



{{< panel style="danger" >}}
Jsonnet source code is available at [github.com/grafana/jsonnet-libs](https://github.com/grafana/jsonnet-libs/tree/master/apache-solr-mixin)
{{< /panel >}}

## Alerts

{{< panel style="warning" >}}
Complete list of pregenerated alerts is available [here](https://github.com/monitoring-mixins/website/blob/master/assets/apache-solr/alerts.yaml).
{{< /panel >}}

### apache-solr

##### ApacheSolrZookeeperChangeInEnsembleSize

{{< code lang="yaml" >}}
alert: ApacheSolrZookeeperChangeInEnsembleSize
annotations:
  description: Zookeeper host {{$labels.zk_host}} has had an ensemble change of {{
    printf "%.0f" $value }} over the last 5 minutes
  summary: Changes in the ZooKeeper ensemble size can affect the stability and performance
    of the cluster.
expr: |
  changes(solr_zookeeper_ensemble_size[5m]) > 0
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### ApacheSolrHighCPUUsageCritical

{{< code lang="yaml" >}}
alert: ApacheSolrHighCPUUsageCritical
annotations:
  description: '{{$labels.instance}} on cluster {{$labels.solr_cluster}} has had a
    system CPU load of {{ printf "%.0f" $value }}%, which is above the threshold of
    85.'
  summary: High CPU load can indicate that Solr nodes are under heavy load, potentially
    impacting performance.
expr: |
  100 * sum without (base_url, item) (avg_over_time(solr_metrics_jvm_os_cpu_load{item="systemCpuLoad"}[5m])) > 85
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### ApacheSolrHighCPUUsageWarning

{{< code lang="yaml" >}}
alert: ApacheSolrHighCPUUsageWarning
annotations:
  description: '{{$labels.instance}} on cluster {{$labels.solr_cluster}} has had a
    system CPU load of {{ printf "%.0f" $value }}%, which is above the threshold of
    75.'
  summary: High CPU load can indicate that Solr nodes are under heavy load, potentially
    impacting performance.
expr: |
  100 * sum without (base_url, item) (avg_over_time(solr_metrics_jvm_os_cpu_load{item="systemCpuLoad"}[5m])) > 75
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### ApacheSolrHighHeapMemoryUsageCritical

{{< code lang="yaml" >}}
alert: ApacheSolrHighHeapMemoryUsageCritical
annotations:
  description: |
    {{$labels.instance}} on cluster {{$labels.solr_cluster}} has had high memory usage of {{ printf "%.0f" $value }}%, which is above the thresold of 75.
  summary: High heap memory usage can lead to garbage collection issues, out-of-memory
    errors, and overall system instability.
expr: |
  100 * sum without(item, base_url)(solr_metrics_jvm_memory_heap_bytes{item="used"}) / clamp_min(sum without(item, base_url)(solr_metrics_jvm_memory_heap_bytes{item="max"}), 1) > 75
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### ApacheSolrHighHeapMemoryUsageWarning

{{< code lang="yaml" >}}
alert: ApacheSolrHighHeapMemoryUsageWarning
annotations:
  description: |
    {{$labels.instance}} on cluster {{$labels.solr_cluster}} has had high memory usage of {{ printf "%.0f" $value }}%, which is above the thresold of 85.
  summary: High heap memory usage can lead to garbage collection issues, out-of-memory
    errors, and overall system instability.
expr: |
  100 * sum without(item, base_url)(solr_metrics_jvm_memory_heap_bytes{item="used"}) / clamp_min(sum without(item, base_url)(solr_metrics_jvm_memory_heap_bytes{item="max"}), 1) > 85
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### ApacheSolrLowCacheHitRatio

{{< code lang="yaml" >}}
alert: ApacheSolrLowCacheHitRatio
annotations:
  description: |
    {{$labels.instance}} on cluster {{$labels.solr_cluster}} has had a low cache hit ratio of {{ printf "%.0f" $value }}% on core {{$labels.core}} of type {{$labels.type}}, which is under the threshold of 75.
  summary: Low cache hit ratios can lead to increased disk I/O and slower query response
    times.
expr: |
  100 * sum without(base_url, category, collection, item, replica, shard) (solr_metrics_core_searcher_cache_ratio{item="hitratio", type=~"documentCache|filterCache|queryResultCache"}) < 75
for: 10m
labels:
  severity: warning
{{< /code >}}
 
##### ApacheSolrHighCoreErrors

{{< code lang="yaml" >}}
alert: ApacheSolrHighCoreErrors
annotations:
  description: |
    {{$labels.instance}} on cluster {{$labels.solr_cluster}} has had a high amount of core errors {{ printf "%.0f" $value }}% on core {{$labels.core}}, which is above the threshold of 15.
  summary: A spike in core errors can indicate serious issues at the core level, affecting
    data integrity and availability.
expr: |
  100 * sum without(base_url, category, collection, handler, replica, shard) (increase(solr_metrics_core_errors_total[10m]) / clamp_min(avg_over_time(solr_metrics_core_errors_total[10m]), 1)) > 15
for: 10m
labels:
  severity: warning
{{< /code >}}
 
##### ApacheSolrHighDocumentIndexing

{{< code lang="yaml" >}}
alert: ApacheSolrHighDocumentIndexing
annotations:
  description: |
    {{$labels.instance}} on cluster {{$labels.solr_cluster}} has had a high document indexing value of {{ printf "%.0f" $value }}% on core {{$labels.core}}, which is above the threshold of 30.
  summary: A sudden spike in document indexing could indicate unintended or malicious
    bulk updates.
expr: |
  100 * sum without(base_url, category, collection, handler, replica, shard) (increase(solr_metrics_core_update_handler_adds_total[15m]) / clamp_min(avg_over_time(solr_metrics_core_update_handler_adds_total[15m]), 1)) > 30
for: 15m
labels:
  severity: warning
{{< /code >}}
 
## Dashboards
Following dashboards are generated from mixins and hosted on github:


- [apache-solr-cluster-overview](https://github.com/monitoring-mixins/website/blob/master/assets/apache-solr/dashboards/apache-solr-cluster-overview.json)
- [apache-solr-logs-overview](https://github.com/monitoring-mixins/website/blob/master/assets/apache-solr/dashboards/apache-solr-logs-overview.json)
- [apache-solr-query-performance](https://github.com/monitoring-mixins/website/blob/master/assets/apache-solr/dashboards/apache-solr-query-performance.json)
- [apache-solr-resource-monitoring](https://github.com/monitoring-mixins/website/blob/master/assets/apache-solr/dashboards/apache-solr-resource-monitoring.json)