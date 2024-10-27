---
title: apache-hbase
---

## Overview



{{< panel style="danger" >}}
Jsonnet source code is available at [github.com/grafana/jsonnet-libs](https://github.com/grafana/jsonnet-libs/tree/master/apache-hbase-mixin)
{{< /panel >}}

## Alerts

{{< panel style="warning" >}}
Complete list of pregenerated alerts is available [here](https://github.com/monitoring-mixins/website/blob/master/assets/apache-hbase/alerts.yaml).
{{< /panel >}}

### apache-hbase-alerts

##### HBaseHighHeapMemUsage

{{< code lang="yaml" >}}
alert: HBaseHighHeapMemUsage
annotations:
  description: The heap memory usage for the JVM on instance {{$labels.instance}}
    in cluster {{$labels.hbase_cluster}} is {{printf "%.0f" $value}} percent, which
    is above the threshold of 80 percent
  summary: There is a limited amount of heap memory available to the JVM.
expr: |
  100 * sum without(context, hostname, processname) (jvm_metrics_mem_heap_used_m{job=~"integrations/apache-hbase"} / clamp_min(jvm_metrics_mem_heap_committed_m{job=~"integrations/apache-hbase"}, 1))  > 80
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### HBaseDeadRegionServer

{{< code lang="yaml" >}}
alert: HBaseDeadRegionServer
annotations:
  description: '{{$value}} RegionServer(s) in cluster {{$labels.hbase_cluster}} are
    unresponsive, which is above the threshold of 0. The name(s) of the dead RegionServer(s)
    are {{$labels.deadregionservers}}'
  summary: One or more RegionServer(s) has become unresponsive.
expr: |
  server_num_dead_region_servers > 0
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### HBaseOldRegionsInTransition

{{< code lang="yaml" >}}
alert: HBaseOldRegionsInTransition
annotations:
  description: '{{printf "%.0f" $value}} percent of RegionServers in transition in
    cluster {{$labels.hbase_cluster}} are transitioning for longer than expected,
    which is above the threshold of 50 percent'
  summary: RegionServers are in transition for longer than expected.
expr: |
  100 * assignment_manager_rit_count_over_threshold / clamp_min(assignment_manager_rit_count, 1) > 50
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### HBaseHighMasterAuthFailRate

{{< code lang="yaml" >}}
alert: HBaseHighMasterAuthFailRate
annotations:
  description: '{{printf "%.0f" $value}} percent of authentication attempts to the
    master are failing in cluster {{$labels.hbase_cluster}}, which is above the threshold
    of 35 percent'
  summary: A high percentage of authentication attempts to the master are failing.
expr: |
  100 * rate(master_authentication_failures[5m]) / (clamp_min(rate(master_authentication_successes[5m]), 1) + clamp_min(rate(master_authentication_failures[5m]), 1)) > 35
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### HBaseHighRSAuthFailRate

{{< code lang="yaml" >}}
alert: HBaseHighRSAuthFailRate
annotations:
  description: '{{printf "%.0f" $value}} percent of authentication attempts to the
    RegionServer {{$labels.instance}} are failing in cluster {{$labels.hbase_cluster}},
    which is above the threshold of 35 percent'
  summary: A high percentage of authentication attempts to a RegionServer are failing.
expr: |
  100 * rate(region_server_authentication_failures[5m]) / (clamp_min(rate(region_server_authentication_successes[5m]), 1) + clamp_min(rate(region_server_authentication_failures[5m]), 1)) > 35
for: 5m
labels:
  severity: warning
{{< /code >}}
 
## Dashboards
Following dashboards are generated from mixins and hosted on github:


- [apache-hbase-cluster-overview](https://github.com/monitoring-mixins/website/blob/master/assets/apache-hbase/dashboards/apache-hbase-cluster-overview.json)
- [apache-hbase-logs](https://github.com/monitoring-mixins/website/blob/master/assets/apache-hbase/dashboards/apache-hbase-logs.json)
- [apache-hbase-regionserver-overview](https://github.com/monitoring-mixins/website/blob/master/assets/apache-hbase/dashboards/apache-hbase-regionserver-overview.json)