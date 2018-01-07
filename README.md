# Grafana dashboard templates for Microsoft Azure

This repository contains a collection of pre-built Grafana dashboard templates for Microsoft Azure resources.  Use the armclient tool to automatically generate Grafana dashboards which you can import into your Grafana server.

For details on the Grafana integration with Azure Monitor, please refer to this article:<br>
https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitor-send-to-grafana

For details on the armclient tool, please refer to the following:<br>
https://github.com/asheniam/armclient

<pre>
usage: armclient grafana &lt;title&gt; &lt;dataSource&gt; &lt;resourcetype&gt; [&lt;maxdashboardresource&gt;] [&lt;maxcontinuation&gt;]

Generate Grafana dashboard JSON files for given Azure resource type.

Flags:
  --help   Show context-sensitive help (also try --help-long and --help-man).
  --config.file="sample-azure.yml"  
           Azure configuration file
  --debug  Debug flag

Args:
  &lt;title&gt;                   This will be used as prefix in the dashboard title
  &lt;dataSource&gt;              The Azure Monitor data source name on Grafana
  &lt;resourcetype&gt;            The Azure Resource Manager (ARM) resource type
  [&lt;maxdashboardresource&gt;]  The max number of Azure resources to include in each dashboard. Default to 10.
  [&lt;maxcontinuation&gt;]       The max number of continuations to follow when calling ARM API. Default to 10.
</pre>
  
