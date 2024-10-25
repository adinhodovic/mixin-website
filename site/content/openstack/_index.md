---
title: openstack
---

## Overview



{{< panel style="danger" >}}
Jsonnet source code is available at [github.com/grafana/jsonnet-libs](https://github.com/grafana/jsonnet-libs/tree/master/openstack-mixin)
{{< /panel >}}

## Alerts

{{< panel style="warning" >}}
Complete list of pregenerated alerts is available [here](https://github.com/monitoring-mixins/website/blob/master/assets/openstack/alerts.yaml).
{{< /panel >}}

### openstack-alerts-openstack

##### OpenStackGlanceIsDown

{{< code lang="yaml" >}}
alert: OpenStackGlanceIsDown
annotations:
  description: OpenStack Glance service is down on cluster {{ $labels.instance }}.
  summary: OpenStack Glance is down.
expr: |
  openstack_glance_up{job=~"integrations/openstack"} == 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackHeatIsDown

{{< code lang="yaml" >}}
alert: OpenStackHeatIsDown
annotations:
  description: OpenStack Heat service is down on cluster {{ $labels.instance }}.
  summary: OpenStack Heat is down.
expr: |
  openstack_heat_up{job=~"integrations/openstack"} == 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackIdentityIsDown

{{< code lang="yaml" >}}
alert: OpenStackIdentityIsDown
annotations:
  description: OpenStack Identity service is down on cluster {{ $labels.instance }}.
  summary: OpenStack Identity is down.
expr: |
  openstack_identity_up{job=~"integrations/openstack"} == 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackPlacementIsDown

{{< code lang="yaml" >}}
alert: OpenStackPlacementIsDown
annotations:
  description: OpenStack Placement service is down on cluster {{ $labels.instance
    }}.
  summary: OpenStack Placement is down.
expr: |
  openstack_placement_up{job=~"integrations/openstack"} == 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackPlacementHighMemoryUsageWarning

{{< code lang="yaml" >}}
alert: OpenStackPlacementHighMemoryUsageWarning
annotations:
  description: |
    OpenStack {{$labels.instance}} is using {{ printf "%.0f" $value }} percent of its allocated memory,
    which is above the threshold of 80 percent.
  summary: OpenStack is using a significant percentage of its allocated memory.
expr: |
  100 * sum by (job,instance) (openstack_placement_resource_usage{job=~"integrations/openstack", resourcetype="MEMORY_MB"})
  /
  (sum by (job,instance) (openstack_placement_resource_total{job=~"integrations/openstack", resourcetype="MEMORY_MB"}) > 0)
  > 80
for: 5m
keep_firing_for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### OpenStackNovaAgentDown

{{< code lang="yaml" >}}
alert: OpenStackNovaAgentDown
annotations:
  description: |
    OpenStack {{$labels.instance}} is using {{ printf "%.0f" $value }} percent of its allocated memory,
    which is above the threshold of 90 percent.
  summary: OpenStack is using a large percentage of its allocated memory, consider
    allocating more resources.
expr: |
  100 * sum by (job,instance) (openstack_placement_resource_usage{job=~"integrations/openstack", resourcetype="MEMORY_MB"})
  /
  (sum by (job,instance) (openstack_placement_resource_total{job=~"integrations/openstack", resourcetype="MEMORY_MB"}) > 0)
  > 90
for: 5m
keep_firing_for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackPlacementHighVCPUUsageWarning

{{< code lang="yaml" >}}
alert: OpenStackPlacementHighVCPUUsageWarning
annotations:
  description: |
    OpenStack {{$labels.instance}} is using {{ printf "%.0f" $value }} percent of its allocated vCPU,
    which is above the threshold of 80 percent.
  summary: OpenStack is using a significant percentage of its allocated vCPU.
expr: |
  100 * sum by (job,instance) (openstack_placement_resource_usage{job=~"integrations/openstack", resourcetype="VCPU"})
  /
  (sum by (job,instance) (openstack_placement_resource_total{job=~"integrations/openstack", resourcetype="VCPU"}) > 0)
  > 80
for: 5m
keep_firing_for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### OpenStackPlacementHighVCPUUsageCritical

{{< code lang="yaml" >}}
alert: OpenStackPlacementHighVCPUUsageCritical
annotations:
  description: |
    OpenStack {{$labels.instance}} is using {{ printf "%.0f" $value }} percent of its allocated vCPU,
    which is above the threshold of 90 percent.
  summary: OpenStack is using a large percentage of its allocated vCPU, consider allocating
    more resources.
expr: |
  100 * sum by (job,instance) (openstack_placement_resource_usage{job=~"integrations/openstack", resourcetype="VCPU"})
  /
  (sum by (job,instance) (openstack_placement_resource_total{job=~"integrations/openstack", resourcetype="VCPU"}) > 0)
  > 90
for: 5m
keep_firing_for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackNeutronHighIPsUsageWarning

{{< code lang="yaml" >}}
alert: OpenStackNeutronHighIPsUsageWarning
annotations:
  description: |
    Network {{$labels.network_name}} is running out of free IP addresses on OpenStack {{$labels.instance}},
    {{ printf "%.0f" $value }} percent of the pool used,
    {{ with printf `sum(openstack_neutron_network_ip_availabilities_total{job=~"integrations/openstack", instance=~"%s", network_name=~"%s"}) - (sum(openstack_neutron_network_ip_availabilities_used{job=~"integrations/openstack", instance=~"%s", network_name=~"%s"}))` .Labels.instance .Labels.network_name .Labels.instance .Labels.network_name | query -}}{{ . | first | value | humanize }}{{ end }} IP addresses available.
  summary: Free IP addresses are running out.
expr: "100 * 
sum by (job,instance, network_name) (openstack_neutron_network_ip_availabilities_used{job=~\"integrations/openstack\",
  network_name=~\".+\"}) 
/
(sum by (job,instance, network_name) (openstack_neutron_network_ip_availabilities_total{job=~\"integrations/openstack\",
  network_name=~\".+\"})
> 0)
> 80
"
for: 5m
keep_firing_for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### OpenStackNeutronHighIPsUsageCritical

{{< code lang="yaml" >}}
alert: OpenStackNeutronHighIPsUsageCritical
annotations:
  description: |
    Network {{$labels.network_name}} is running out of free IP addresses on OpenStack {{$labels.instance}},
    {{ printf "%.0f" $value }} percent of the pool used,
    {{ with printf `sum(openstack_neutron_network_ip_availabilities_total{job=~"integrations/openstack", instance=~"%s", network_name=~"%s"}) - (sum(openstack_neutron_network_ip_availabilities_used{job=~"integrations/openstack", instance=~"%s", network_name=~"%s"}))` .Labels.instance .Labels.network_name .Labels.instance .Labels.network_name | query -}}{{ . | first | value | humanize }}{{ end }} IP addresses available.
  summary: There are practically no free IP addresses left.
expr: "100 * 
sum by (job,instance, network_name) (openstack_neutron_network_ip_availabilities_used{job=~\"integrations/openstack\",
  network_name=~\".+\"}) 
/
(sum by (job,instance, network_name) (openstack_neutron_network_ip_availabilities_total{job=~\"integrations/openstack\",
  network_name=~\".+\"})
> 0)
> 90
"
for: 5m
keep_firing_for: 5m
labels:
  severity: critical
{{< /code >}}
 
### openstack-nova-alertsopenstack

##### OpenStackNovaIsDown

{{< code lang="yaml" >}}
alert: OpenStackNovaIsDown
annotations:
  description: OpenStack Nova is down on {{ $labels.instance }}.
  summary: OpenStack Nova service is down.
expr: |
  openstack_nova_up{job=~"integrations/openstack"} == 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackNovaAgentIsDown

{{< code lang="yaml" >}}
alert: OpenStackNovaAgentIsDown
annotations:
  description: An OpenStack Nova agent is down on hostname {{ $labels.hostname }}
    on OpenStack cluster {{ $labels.instance }}.
  summary: OpenStack Nova agent is down on the specific node.
expr: |
  openstack_nova_agent_state{job=~"integrations/openstack",adminState="enabled"} != 1
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackNovaHighVMMemoryUsage

{{< code lang="yaml" >}}
alert: OpenStackNovaHighVMMemoryUsage
annotations:
  description: |
    Virtual machines on OpenStack {{ $labels.instance }} are using {{ printf "%.0f" $value }} percent of their allocated memory,
    which is above the threshold of 80 percent.
  summary: VMs are using a high percentage of their allocated memory.
expr: |
  100 * openstack_nova_limits_memory_used{job=~"integrations/openstack"} / (openstack_nova_limits_memory_max{job=~"integrations/openstack"} > 0) > 80
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### OpenStackNovaHighVMVCPUUsage

{{< code lang="yaml" >}}
alert: OpenStackNovaHighVMVCPUUsage
annotations:
  description: |
    Virtual machines on OpenStack {{$labels.instance}} are using {{ printf "%.0f" $value }} percent of their allocated virtual CPUs,
    which is above the threshold of 80 percent.
  summary: VMs are using a high percentage of their allocated virtual CPUs.
expr: |
  100 * openstack_nova_limits_vcpus_used{job=~"integrations/openstack"} / (openstack_nova_limits_vcpus_max{job=~"integrations/openstack"} > 0) > 80
for: 5m
labels:
  severity: warning
{{< /code >}}
 
### openstack-neutron-alertsopenstack

##### OpenStackNeutronIsDown

{{< code lang="yaml" >}}
alert: OpenStackNeutronIsDown
annotations:
  description: OpenStack Neutron service is down on cluster {{ $labels.instance }}.
  summary: OpenStack Neutron is down.
expr: |
  openstack_neutron_up{job=~"integrations/openstack"} == 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackNeutronAgentIsDown

{{< code lang="yaml" >}}
alert: OpenStackNeutronAgentIsDown
annotations:
  description: |
    OpenStack Neutron agent`s service {{ $labels.service }} is down on hostname {{ $labels.hostname }} on OpenStack cluster {{ $labels.instance }}.
    If {{ $labels.service }} is no longer required on this host, disable it administratively by running:
    OpenStack network agent set {{ $labels.id }} --disable
  runbook_url: https://docs.openstack.org/neutron/zed/admin/config-services-agent.html#agent-s-admin-state-specific-config-options
  summary: OpenStack Neutron agent is down on the specific node.
expr: |
  openstack_neutron_agent_state{job=~"integrations/openstack",adminState="up"} != 1
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackNeutronL3AgentIsDown

{{< code lang="yaml" >}}
alert: OpenStackNeutronL3AgentIsDown
annotations:
  description: OpenStack Neutron L3 agent is down on hostname {{ $labels.agent_host
    }} on OpenStack cluster {{ $labels.instance }}.
  summary: OpenStack Neutron L3 agent is down on the specific node.
expr: |
  openstack_neutron_l3_agent_of_router{job=~"integrations/openstack",agent_admin_up="true"} != 1
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackNeutronHighDisconnectedPortRate

{{< code lang="yaml" >}}
alert: OpenStackNeutronHighDisconnectedPortRate
annotations:
  description: |
    {{ printf "%.0f" $value }} percent of ports managed by the Neutron service on OpenStack cluster {{$labels.instance}} have no IP addresses assigned to them,
    which is above the threshold of 25.
  summary: A high rate of ports have no IP addresses assigned to them.
expr: |
  100 * openstack_neutron_ports_no_ips{job=~"integrations/openstack"} / clamp_min(openstack_neutron_ports{job=~"integrations/openstack"}, 1) > 25
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackNeutronHighInactiveRouterRate

{{< code lang="yaml" >}}
alert: OpenStackNeutronHighInactiveRouterRate
annotations:
  description: |
    {{ printf "%.0f" $value }} percent of routers managed by the Neutron service on cluster {{$labels.instance}} are currently inactive,
    which is above the threshold of 15.
  summary: A high rate of routers are currently inactive.
expr: |
  100 * openstack_neutron_routers_not_active{job=~"integrations/openstack"} / clamp_min(openstack_neutron_routers{job=~"integrations/openstack"}, 1) > 15
for: 5m
labels:
  severity: critical
{{< /code >}}
 
### openstack-cinder-alertsopenstack

##### OpenStackCinderIsDown

{{< code lang="yaml" >}}
alert: OpenStackCinderIsDown
annotations:
  description: OpenStack Cinder service is down on cluster {{ $labels.instance }}.
  summary: OpenStack Cinder is down.
expr: |
  openstack_cinder_up{job=~"integrations/openstack"} == 0
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackCinderAgentIsDown

{{< code lang="yaml" >}}
alert: OpenStackCinderAgentIsDown
annotations:
  description: OpenStack Cinder agent is down on hostname {{ $labels.hostname }} on
    OpenStack cluster {{ $labels.instance }}.
  summary: OpenStack Cinder agent is down on the specific node.
expr: |
  openstack_cinder_agent_state{job=~"integrations/openstack",adminState="enabled"} != 1
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### OpenStackCinderHighPoolCapacityUsage

{{< code lang="yaml" >}}
alert: OpenStackCinderHighPoolCapacityUsage
annotations:
  description: |
    Pools managed by the Cinder service on cluster {{$labels.instance}} are using {{ printf "%.0f" $value }} percent of their allocated capacity,
    which is above the threshold of 80 percent.
  summary: Cinder pools are using a large amount of their maximum capacity.
expr: |
  100 * (openstack_cinder_pool_capacity_total_gb{job=~"integrations/openstack"} - openstack_cinder_pool_capacity_free_gb{job=~"integrations/openstack"}) / clamp_min(openstack_cinder_pool_capacity_total_gb{job=~"integrations/openstack"}, 1) > 80
for: 10m
labels:
  severity: warning
{{< /code >}}
 
##### OpenStackCinderHighVolumeMemoryUsage

{{< code lang="yaml" >}}
alert: OpenStackCinderHighVolumeMemoryUsage
annotations:
  description: |
    Volumes managed by the Cinder service on cluster {{$labels.instance}} are using {{ printf "%.0f" $value }} percent of their allocated memory,
    which is above the threshold of 80 percent.
  summary: Cinder volumes are using a large amount of their maximum memory.
expr: |
  100 * openstack_cinder_limits_volume_used_gb{job=~"integrations/openstack"} / (openstack_cinder_limits_volume_max_gb{job=~"integrations/openstack"} > 0) > 80
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### OpenStackCinderHighBackupMemoryUsage

{{< code lang="yaml" >}}
alert: OpenStackCinderHighBackupMemoryUsage
annotations:
  description: |
    Backups managed by the Cinder service on cluster {{$labels.instance}} are using {{ printf "%.0f" $value }} percent of their allocated memory,
    which is above the threshold of 80 percent.
  summary: Cinder backups are using a large amount of their maximum memory.
expr: |
  100 * openstack_cinder_limits_backup_used_gb{job=~"integrations/openstack"} / (openstack_cinder_limits_backup_max_gb{job=~"integrations/openstack"} > 0) > 80
for: 5m
labels:
  severity: warning
{{< /code >}}
 
## Dashboards
Following dashboards are generated from mixins and hosted on github:


- [*](https://github.com/monitoring-mixins/website/blob/master/assets/openstack/dashboards/*.json)
