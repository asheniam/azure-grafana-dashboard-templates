# Grafana dashboard templates for Microsoft Azure

This repository contains a collection of pre-built Grafana dashboard templates for Microsoft Azure resources.  Use the armclient tool to automatically generate Grafana dashboards which you can import into your Grafana server.

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
  
<b>Prerequisite:</b>

This assumes that you are familiar with Grafana and the Azure Monitor data source plugin.  For more information, please refer to the following article:

https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitor-send-to-grafana

<b>How to use:</b>

1) Create a service principal which has Reader permission to access your Azure subscription.
https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal

2) Create armclient config file with service principal credentials.

Example: sample-azure.yml
<pre>
Credentials:
  environment: public
  subscription_id: &lt;subscriptionId&gt;
  client_id: &lt;clientId&gt;
  client_secret: &lt;clientSecret&gt;
  tenant_id: &lt;tenantId&gt;
</pre>

3) Run armclient command line tool to generate Grafana dashboard JSON files.

You will need the name of the Azure Monitor data source on your Grafana server and the ARM resource type that you want to generate dashboards.

Example: ./armclient --config.file=sample-azure.yml grafana production AzureMonitorDataSource microsoft.storage/storageaccounts

- production = This string is added to the title of the generated Grafana dashboard<br>
- AzureMonitorDataSource = This is the name of the Azure Monitor data source on your Grafana server<br>
- microsoft.storage/storageaccounts = This is the Azure Resource Manager (ARM) resource type<br>

The armclient tool will generate dashboard JSON files -- one for each region and one for all regions.

In the above example the following JSON files could be generated:

dashboard_production_microsoft_storage_storageaccounts_allregions.json		
dashboard_production_microsoft_storage_storageaccounts_eastus.json		
dashboard_production_microsoft_storage_storageaccounts_southcentralus.json	
dashboard_production_microsoft_storage_storageaccounts_westcentralus.json	

4) Import the dashboard JSON files into your Grafana server.

5) Enjoy!

<b>How to contribute</b>

The armclient automatically pulls dashboard templates from this GitHub repository.  To contribute new dashboard templates, please following the following instructions.

1) On your Grafana server, create a dashboard for a given Azure Resource Manager (ARM) resource type.
Note: In your dashboard, you should only select single Azure resource for each of the charts.  The armclient tool will automatically replace all the Azure Monitor targets in the dashboard JSON using ARM resource IDs from the given Azure subscription.

2) Export the dashboard to JSON. Save the JSON into a file called template.json.

3) Anonymize the contents of template.json
- Replace the data source name with {dataSourceName}<br>
- Replace the resource group name with {resourceGroupName}<br>
- Replace the resource name with {resourceName}<br>

4) Create the following directory structure: &lt;ARM resource type&gt;/&lt;dashboard friendly name&gt;
  
For example, if you are creating a dashboard specific to Azure Storage account latencies, you could create the following directory structure:

microsoft.storage/storageaccounts/latency

5) In the new directory, create the following 3 files:
- template.json : This is the dashboard template from step #3
- dashboard.png : This is a sample screenshot of the dashboard.
- README.md : This README contains the dashboard screenshot.

6) Submit a pull request.

7) After the pull request is merged, anyone who runs ./armclient grafana command for the given ARM resource type will generate dashboards using your template.
