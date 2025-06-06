---
id: couchbase
title: Couchbase - Classic Collector
sidebar_label: Couchbase
description: Couchbase is a distributed document database with a powerful search engine and in-built operational and analytical capabilities.
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<img src={useBaseUrl('img/integrations/databases/couchbase-logo.png')} alt="Thumbnail icon" width="75"/>

Couchbase, a modern database for enterprise applications, is a distributed document database with a powerful search engine and in-built operational and analytical capabilities. It brings the power of NoSQL to the edge and provides fast, efficient bidirectional synchronization of data between the edge and the cloud.

The Sumo Logic app for Couchbase helps you monitor activity in Couchbase. The pre-configured dashboards provide insight into the Health of the Cluster, the Status of the Buckets, I/O of Reading/Writing, Errors, the Events of Couchbase Servers that help you understand your clusters.

This app has been tested with the following Couchbase with Telegraf versions:
* Kubernetes: Couchbase version: 7.0.2 - enterprise with Telegraf version 1.21.1
* Non-Kubernetes: Couchbase version: 7.0.2 - enterprise with Telegraf version 1.21.1

:::note
Telegraf 1.14 default of Kubernetes Collection will not work.
:::

## Collecting logs and metrics for the Couchbase app

This section provides instructions for configuring log and metric collection for the Sumo Logic app for Couchbase.

### Configure Collection for Couchbase

Sumo Logic supports the collection of logs and metrics data from Couchbase in both Kubernetes and non-Kubernetes environments. Click on the appropriate tab below based on the environment where your Couchbase clusters are hosted.

<Tabs
  groupId="k8s-nonk8s"
  defaultValue="k8s"
  values={[
    {label: 'Kubernetes environments', value: 'k8s'},
    {label: 'Non-Kubernetes environments', value: 'non-k8s'},
  ]}>

<TabItem value="k8s">

The following diagram illustrates how data is collected from Couchbase in Kubernetes environments. There are four services that make up the metric collection pipeline: Telegraf, Telegraf Operator, Prometheus, and [Sumo Logic Distribution for OpenTelemetry Collector](https://github.com/SumoLogic/sumologic-otel-collector).

<img src={useBaseUrl('img/integrations/databases/couchbase1.png')} alt="couchbase1" />

The first service in the metrics pipeline is Telegraf. Telegraf collects metrics from Couchbase. Note that we’re running Telegraf in each pod we want to collect metrics from as a sidecar deployment that is Telegraf runs in the same pod as the containers it monitors. Telegraf uses the [Couchbase input plugin](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/couchbase) to obtain metrics. For simplicity, the diagram doesn’t show the input plugins.
The injection of the Telegraf sidecar container is done by the Telegraf Operator.
Prometheus pulls metrics from Telegraf and sends them to [Sumo Logic Distribution for OpenTelemetry Collector](https://github.com/SumoLogic/sumologic-otel-collector) which enriches metadata and sends metrics to Sumo Logic.

In the logs pipeline, Sumo Logic Distribution for OpenTelemetry Collector collects logs written to standard out and forwards them to another instance of Sumo Logic Distribution for OpenTelemetry Collector, which enriches metadata and sends logs to Sumo Logic.

:::note Prerequisites
It’s assumed that you are using the latest helm chart version if not, upgrade using the instructions [here](/docs/send-data/kubernetes). When you upgrade the helm chart, you must upgrade telegraf version to 1.21.1 by adding the statement below in the upgrade command helm chart:
```bash
--set telegraf-operator.image.sidecarImage=telegraf:1.21.1
```
:::

#### Configure Metrics Collection

To collect Couchbase metrics from a Kubernetes environment, we use the Telegraf Operator, which is packaged with our Kubernetes collection. You can learn more about this [here](/docs/send-data/collect-from-other-data-sources/collect-metrics-telegraf/telegraf-collection-architecture).

1. [Set up Kubernetes Collection with the Telegraf Operator](/docs/send-data/collect-from-other-data-sources/collect-metrics-telegraf/install-telegraf).
2. On your Couchbase Pods, add the following annotations:
```sql
annotations:
  telegraf.influxdata.com/class: sumologic-prometheus
  prometheus.io/scrape: "true"
  prometheus.io/port: "9273"
  telegraf.influxdata.com/inputs: |+
[[inputs.couchbase]]
  servers = ["http://<USER_TO_BE_CHANGED>:<PASS_TO_BE_CHANGED>@A@localhost:8091"]
  bucket_stats_included = ["*"]
  [inputs.couchbase.tags]
    db_cluster ="ENV_TO_BE_CHANGED"--If you haven’t defined a cluster in Couchbase, enter `default`
    component ="database"
    environment ="ENV_TO_BE_CHANGED"
    db_system ="couchbase"
    db_cluster_address = "ENV_TO_BE_CHANGED"
    db_cluster_port = "ENV_TO_BE_CHANGED"
```
3. Enter in values for the following parameters (marked `ENV_TO_BE_CHANGED` above):
  * `telegraf.influxdata.com/inputs`. This contains the required configuration for the Telegraf Couchbase Input plugin. Refer to [this doc](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/couchbase) for more information on configuring the Couchbase input plugin for Telegraf. Note: As telegraf will be run as a sidecar, the host should always be localhost.
  * In the Input plugins section (`[[inputs.couchbase]]`):
     * `servers`: This is the endpoint of the management portal of couchbase server. For detail, see this [doc](https://docs.couchbase.com/server/current/manage/manage-ui/manage-ui.html) .
  * In the tags section (`[inputs.couchbase.tags]`):
     * `environment`. This is the deployment environment where the Couchbase cluster identified by the value of **servers** resides. For example: dev, prod or qa. While this value is optional we highly recommend setting it.
     * `db_cluster`. Enter a name to identify this Couchbase cluster. This cluster name will be shown in the Sumo Logic dashboards.  
     * `db_cluster_address` - Enter the cluster hostname or ip address that is used by the application to connect to the database. It could also be the load balancer or proxy endpoint.
     * `db_cluster_port` - Enter the database port. If not provided, a default port will be used.

     :::note
     `db_cluster_address` and `db_cluster_port` should reflect exact configuration of DB client configuration in your application, especially if you instrument it with OT tracing. The values of these fields should match exactly the connection string used by the database client (reported as values for `net.peer.name` and `net.peer.port` metadata fields).
     For example, if your application uses “couchbase-prod.sumologic.com:3306” as the connection string, the field values should be set as follows: `db_cluster_address=couchbase-prod.sumologic.com db_cluster_port=3306`

     If your application connects directly to a given Couchbase node, rather than the whole cluster, use the application connection string to override the value of the “host” field in the Telegraf configuration: `host=couchbase-prod.sumologic.com`

     Pivoting to Tracing data from Entity Inspector is possible only for “Couchbase address” Entities.
     :::
  * **Do not modify the following values** as it will cause the Sumo Logic apps to not function correctly.
    * `telegraf.influxdata.com/class: sumologic-prometheus`. This instructs the Telegraf operator what output to use. This should not be changed.
    * `prometheus.io/scrape: "true"`. This ensures our Prometheus will scrape the metrics.
    * `prometheus.io/port: "9273"`. This tells prometheus what ports to scrape on. This should not be changed.
    * `telegraf.influxdata.com/inputs`.- In the tags section (`[inputs.couchbase.tags]`):
      * `component: “database”` - This value is used by Sumo Logic apps to identify application components.
      * `db_system: “couchbase”` - This value identifies the database system.
  * See [this doc](/docs/send-data/collect-from-other-data-sources/collect-metrics-telegraf/install-telegraf#configuring-telegraf) for more parameters that can be configured in the Telegraf agent globally.
4. Sumo Logic Kubernetes collection will automatically start collecting metrics from the pods having the labels and annotations defined in the previous step.
5. Verify metrics in Sumo Logic.

<br/>

#### Configure Logs Collection

This section explains the steps to collect Couchbase logs from a Kubernetes environment.

1. **Add labels on your Couchbase pods to capture logs from standard output on Kubernetes (recommended)**.
   1. Apply following labels to the Couchbase pod:
    ```sql
    environment = "prod_CHANGEME"
    component = "database"
    db_system = "couchbase"
    db_cluster = "<cluster_CHANGEME>"
    db_cluster_address: <your cluster’s hostname or ip address or service endpoint>
    db_cluster_port: <database port>
    ```
   2. Enter in values for the following parameters (marked `CHANGE_ME` above):
    * `environment`. This is the deployment environment where the Couchbase cluster identified by the value of **servers** resides. For example:- dev, prod, or QA. While this value is optional we highly recommend setting it.
    * `db_cluster`. Enter a name to identify this Couchbase cluster. This cluster name will be shown in the Sumo Logic dashboards. If you haven’t defined a cluster in Couchbase, then enter `default` for `db_cluster`.
    * `db_cluster_address` - Enter the cluster hostname or ip address that is used by the application to connect to the database. It could also be the load balancer or proxy endpoint.
    * `db_cluster_port` - Enter the database port. If not provided, a default port will be used.
    :::note
    `db_cluster_address` and `db_cluster_port` should reflect exact configuration of DB client configuration in your application, especially if you instrument it with OT tracing. The values of these fields should match exactly the connection string used by the database client (reported as values for `net.peer.name` and `net.peer.port` metadata fields).
    For example, if your application uses `“couchbase-prod.sumologic.com:3306”` as the connection string, the field values should be set as follows: `db_cluster_address=couchbase-prod.sumologic.com db_cluster_port=3306`.

    If your application connects directly to a given Couchbase node, rather than the whole cluster, use the application connection string to override the value of the “host” field in the Telegraf configuration: `host=couchbase-prod.sumologic.com`.

    Pivoting to Tracing data from Entity Inspector is possible only for “Couchbase address” Entities.
    :::
    * **Do not modify the following values** as it will cause the Sumo Logic apps to not function correctly.
     * `component: “database”` - This value is used by Sumo Logic apps to identify application components.
     * `db_system: “couchbase”` - This value identifies the database system.
     See [this doc](/docs/send-data/collect-from-other-data-sources/collect-metrics-telegraf/install-telegraf#configuring-telegraf) for more parameters that can be configured in the Telegraf agent globally.
   3. The Sumologic-Kubernetes-Collection will automatically capture the logs from stdout and will send the logs to Sumologic. For more information on deploying Sumologic-Kubernetes-Collection, [visit](/docs/integrations/containers-orchestration/kubernetes#collecting-metrics-and-logs-for-the-kubernetes-app) here.
   4. Verify logs in Sumo Logic.
2. **Collecting Couchbase Logs from a Log File on Kubernetes (optional)**.
   1. Determine the location of the Couchbase log file on Kubernetes. This can be determined from the config file /opt/couchbase/etc/couchbase/static_config squid.conf for your Couchbase cluster along with the mounts on the Couchbase pods.
   2. Install the Sumo Logic [tailing sidecar operator](https://github.com/SumoLogic/tailing-sidecar/tree/main/operator#deploy-tailing-sidecar-operator).
   3. Add the following annotation in addition to the existing annotations.
    ```xml
    annotations:
      tailing-sidecar: sidecarconfig;<mount>:<path_of_Couchbase_log_file>/<Couchbase_log_file_name>
    ```
    Example:
    ```sql
    annotations:
      tailing-sidecar: sidecarconfig;data:/opt/couchbase/var/lib/couchbase/logs/audit.log
    ```
   4. Make sure that the Couchbase pods are running and annotations are applied by using the command:
    ```bash
    kubectl describe pod <Couchbase_pod_name>
    ```
   5. Sumo Logic Kubernetes collection will automatically start collecting logs from the pods having the annotations defined above.
   6. Verify logs in Sumo Logic.

<br/>**FER to normalize the fields in Kubernetes environments.** Labels created in Kubernetes environments automatically are prefixed with pod_labels. To normalize these for our app to work, a Field Extraction Rule named **AppObservabilityCouchbaseDatabaseFER** is automatically created for Database Application Components.
<br/>

</TabItem>
<TabItem value="non-k8s">

For non-Kubernetes environments, we use the Telegraf operator for Couchbase metric collection and the [Installed Collector](/docs/send-data/installed-collectors) for collecting Couchbase logs. The diagram below illustrates the components of the Couchbase collection in a non-Kubernetes environment. Telegraf uses the [Couchbase input plugin](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/couchbase) to obtain Couchbase metrics and the Sumo Logic output plugin to send the metrics to Sumo Logic. Logs from Couchbase are collected by a [Local File Source](/docs/send-data/installed-collectors/sources/local-file-source).<img src={useBaseUrl('img/integrations/databases/couchbase2.png')} alt="couchbase2" />

The process to set up collection for Couchbase data is done through the following steps.

#### Configure Logs Collection

The Sumo Logic Couchbase app supports the audit log, query log, error log, access log. For details, [refer to Couchbase logging documentation](https://docs.couchbase.com/server/current/manage/manage-logging/manage-logging.html#changing-log-file-locations).

1. **Configure logging in Couchbase**. By default, the Couchbase will write the log to the log directory that was configured during installation. For example, on Linux, the log directory would be `/opt/couchbase/var/lib/couchbase/logs`. By default, the Audit log is disabled, you must enable the audit log following [these instructions](https://docs.couchbase.com/server/current/manage/manage-security/manage-auditing.html). Query log, error log, the access log will be enabled by default.
2. **Configure a Collector**. Use one of the following Sumo Logic Collector options:
   1. To collect logs directly from the Couchbase machine, configure an [Installed Collector](/docs/send-data/installed-collectors).
   2. If you're using a service like Fluentd, or you would like to upload your logs manually, configure a [Hosted Collector](/docs/send-data/hosted-collectors/configure-hosted-collector).
3. **Configure a local file source**. Choose one of the options:

<details>
<summary>3A. <strong>For an Installed Collector</strong> (click to expand)</summary>

To collect logs directly from your Couchbase machine, use an Installed Collector and Multi Local File Source. Repeat the below steps for each log source: audit log, query log, error log, access log.

1. Add a [Local File Source](/docs/send-data/installed-collectors/sources/local-file-source).
2. Configure the Local File Source fields as follows:
  * **Name**. (Required)
  * **Description**. (Optional)
  * **File Path (Required)**. Enter the path to your log files: The files are typically located in folder /opt/couchbase/var/lib/couchbase/logs.
    * For Audit Log: /opt/couchbase/var/lib/couchbase/logs/audit.log
    * For Error Log: /opt/couchbase/var/lib/couchbase/logs/error.log
    * For Access Log: /opt/couchbase/var/lib/couchbase/logs/http_access.log
    * For Query Log: /opt/couchbase/var/lib/couchbase/logs/query.log
  * **Source Host**. Sumo Logic uses the hostname assigned by the OS unless you enter a different hostname.
  * **Source Category**. Enter any string to tag the output collected from this Source, such as Couchbase/AccessLog for access log. (The Source Category metadata field is a fundamental building block to organize and label Sources. For details, see [Best Practices](/docs/send-data/best-practices).)
  * **Fields.** Set the following fields
     ```sql
     component = database
     db_system = couchbase
     db_cluster = <Your_Couchbase_Cluster_Name>
     ```
    Enter **Default** if you do not have one. `<environment = <Your_Environment_Name>` (for example, Dev, QA, or Prod).
  * `db_cluster_address` - Enter the cluster hostname or ip address that is used by the application to connect to the database. It could also be the load balancer or proxy endpoint.
  * `db_cluster_port` - Enter the database port. If not provided, a default port will be used.
  :::note
  `db_cluster_address` and `db_cluster_port` should reflect the exact configuration of DB client configuration in your application, especially if you instrument it with OT tracing. The values of these fields should match exactly the connection string used by the database client (reported as values for the `net.peer.name` and `net.peer.port` metadata fields).
  For example, if your application uses `“couchbase-prod.sumologic.com:3306”` as the connection string, the field values should be set as follows: `db_cluster_address=couchbase-prod.sumologic.com db_cluster_port=3306`
  If your application connects directly to a given Couchbase node, rather than the whole cluster, use the application connection string to override the value of the “host” field in the Telegraf configuration: `host=couchbase-prod.sumologic.com`
   Pivoting to Tracing data from Entity Inspector is possible only for “Couchbase address” Entities.
   :::
3. Configure the **Advanced **section:
    * **Enable Timestamp Parsing**. Select Extract timestamp information from log fileentries.
    * **Time Zone**. Automatically detect.
    * **Timestamp Format**. The timestamp format is automatically detected.
    * **Encoding**. Select UTF-8 (Default).
    * **Enable Multiline Processing**.
    * **Error logs**. Select **Detect messages spanning multiple lines** and **Infer Boundaries - Detect message boundaries automatically**.
    * **Access logs.** These are single-line logs, uncheck **Detect messages spanning multiple lines**.
4. Click **Save**.

</details>

<details>
<summary>3B. <strong>For a Hosted Collector</strong> (click to expand)</summary>

If you're using a service like Fluentd, or you would like to upload your logs manually, use a Hosted Collector and an HTTP Source.

1. Add an [HTTP Source](/docs/send-data/hosted-collectors/http-source/logs-metrics).
2. Configure the HTTP Source fields as follows:
* **Name**. (Required)
* **Description**. (Optional)
* **Source Host**. Sumo Logic uses the hostname assigned by the OS unless you enter a different hostname.
* **Source Category**. Enter any string to tag the output collected from this Source, such as Couchbase/AccessLog for access log. (The Source Category metadata field is a fundamental building block to organize and label Sources. For details, see [Best Practices](/docs/send-data/best-practices).)
* Fields. Set the following fields:
   * `component = database`
   * `db_system = couchbase`
   * `db_cluster = <Your_Couchbase_Cluster_Name>`
   * `environment = <Environment_Name>`, such as Dev, QA or Prod.
   * `db_cluster_address` - Enter the cluster hostname or ip address that is used by the application to connect to the database. It could also be the load balancer or proxy endpoint.
   * `db_cluster_port` - Enter the database port. If not provided, a default port will be used.
3. Configure the **Advanced **section:
* **Enable Timestamp Parsing**. Select **Extract timestamp information from log file entries**.
* **Time Zone**. For Access logs, use the time zone from the log file. For Error logs, make sure to select the correct time zone.
* **Timestamp Format**. The timestamp format is automatically detected.
* **Enable Multiline Processing**.
    * **Error logs**: Select **Detect messages spanning multiple lines** and **Infer Boundaries - Detect message boundaries automatically**.
    * **Access logs**: These are single-line logs, uncheck **Detect messages spanning multiple lines**.
4. Click **Save**.
5. When the URL associated with the HTTP Source is displayed, copy the URL so you can add it to the service you are using, such as Fluentd.

</details>

<br/>

#### Configure Metrics Collection

1. To set up a Sumo Logic HTTP Source, you'll need to configure a Hosted Collector for Metrics. To create a new Sumo Logic hosted collector, perform the steps in [Configure a Hosted Collector](/docs/send-data/hosted-collectors/configure-hosted-collector).
2. **Configure an HTTP Logs and Metrics source**:
    1. On the created Hosted Collector on the Collection Management screen, select **Add Source**.
    2. Select **HTTP Logs & Metrics.**
        1. **Name.** (Required). Enter a name for the source.
        2. **Description.** (Optional).
        3. **Source Category** (Recommended). Be sure to follow the [Best Practices for Source Categories](/docs/send-data/best-practices). A recommended Source Category may be Prod/DBServer/Couchbase/Metrics.
    3. Select **Save**.
    4. Take note of the URL provided once you click _Save_. You can retrieve it again by selecting the **Show URL** next to the source on the Collection Management screen.
3. **Install Telegraf, if you haven’t already**, using the [following steps](/docs/send-data/collect-from-other-data-sources/collect-metrics-telegraf/install-telegraf.md).
2. **Configure and start Telegraf.** As part of collecting metrics data from Telegraf, we will use the[ Couchbase input plugin](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/couchbase) to get data from Telegraf and the [Sumo Logic output plugin](https://github.com/SumoLogic/fluentd-output-sumologic) to send data to Sumo Logic. Create or modify `telegraf.conf` and copy and paste the text below:
```sql
[[inputs.couchbase]]
  servers = ["http://<USER_TO_BE_CHANGED>:<PASS_TO_BE_CHANGED>@localhost:8091"]
  bucket_stats_included = ["*"]
  [inputs.couchbase.tags]
    db_cluster ="<ClusterName_TO_BE_CHANGED>"
    component ="database"
    environment ="<env_TO_BE_CHANGED>"
    db_system ="couchbase"
    db_cluster_address = "ENV_TO_BE_CHANGED"
    db_cluster_port = "ENV_TO_BE_CHANGED"
[[outputs.sumologic]]
  url = "<URL_from_HTTP_Logs_and_Metrics_Source>"
  data_format = "prometheus"
```

  Enter values for fields annotated with `<_TO_BE_CHANGED>` to the appropriate values. Do not include the brackets (`< >`) in your final configuration.
  * Input plugins section, which is `[[inputs.couchbase]]`:
     * `servers`: This is the endpoint of the management portal of couchbase server. For details, see this [doc](https://docs.couchbase.com/server/current/manage/manage-ui/manage-ui.html) .
  * In the tags section (`[inputs.couchbasesnmp.tags]`):
     * `environment`. This is the deployment environment where the Couchbase server identified by the value of **`servers`** resides. For example; dev, prod, or QA. While this value is optional we highly recommend setting it.
  * `db_cluster`. Enter a name to identify this Couchbase cluster. This cluster name will be shown in our dashboards. If you haven’t defined a cluster in Couchbase, then enter ‘default’ for this.
  * `db_cluster_address` - Enter the cluster hostname or ip address that is used by the application to connect to the database. It could also be the load balancer or proxy endpoint.
  * `db_cluster_port` - Enter the database port. If not provided, a default port will be used.

  :::note
  `db_cluster_address` and `db_cluster_port` should reflect the exact configuration of DB client configuration in your application, especially if you instrument it with OT tracing. The values of these fields should match exactly the connection string used by the database client (reported as values for net.peer.name and net.peer.port metadata fields).

  For example, if your application uses `“couchbase-prod.sumologic.com:3306”` as the connection string, the field values should be set as follows: `db_cluster_address=couchbase-prod.sumologic.com db_cluster_port=3306`.

  If your application connects directly to a given Couchbase node, rather than the whole cluster, use the application connection string to override the value of the “host” field in the Telegraf configuration: `host=couchbase-prod.sumologic.com`

  Pivoting to Tracing data from Entity Inspector is possible only for “Couchbase address” Entities.
  :::
  * In the output plugins section (`[[outputs.sumologic]]`):
    * `url` - This is the HTTP source URL created previously. See [this doc](/docs/send-data/collect-from-other-data-sources/collect-metrics-telegraf/configure-telegraf-output-plugin.md) for more information on additional parameters for configuring the Sumo Logic Telegraf output plugin.

**Do not modify** the following values set by this Telegraf configuration as it will cause the Sumo Logic app to not function correctly.
* `data_format: “prometheus”`. In the output `[[outputs.sumologic]]` plugins section. Metrics are sent in the Prometheus format to Sumo Logic.
* `component - “database”` - In the input `[[inputs.couchbase]]` plugins section. This value is used by Sumo Logic apps to identify application components.
* `db_system- “couchbase”` - In the input plugins sections. This value identifies the database system.

See [this doc](https://github.com/influxdata/telegraf/blob/master/etc/logrotate.d/telegraf) for all other parameters that can be configured in the Telegraf agent globally.

After you have finalized your `telegraf.conf` file, you can start or reload the telegraf service using instructions from this[ doc](https://docs.influxdata.com/telegraf/v1.17/introduction/getting-started/#start-telegraf-service).

At this point, Telegraf should start collecting the Couchbase metrics and forward them to the Sumo Logic HTTP Source.

</TabItem>
</Tabs>

## Installing the Couchbase app

This section demonstrates how to install the Couchbase app.

import AppInstall2 from '../../reuse/apps/app-install-only-k8s.md';

<AppInstall2 />


As part of the app installation process, the following fields will be created by default:
* `component`
* `environment`
* `db_system`
* `db_cluster`
* `pod`
* `db_cluster_address`
* `db_cluster_port`

Additionally, if you're using Couchbase in the Kubernetes environment, the following additional fields will be created by default during the app installation process:
* `pod_labels_component`
* `pod_labels_environment`
* `pod_labels_db_system`
* `pod_labels_db_cluster`
* `pod_labels_db_cluster_address`
* `pod_labels_db_cluster_port`

For information on setting up fields, see [Fields](/docs/manage/fields).

## Viewing Couchbase Dashboards

import ViewDashboards from '../../reuse/apps/view-dashboards.md';

<ViewDashboards/>

### Overview

The **Couchbase - Overview** dashboard provides an at-a-glance view of the health of the Couchbase clusters and servers, performance, and problems causing errors.

Use this dashboard to:
* Gain insights into information about the number of nodes, number of buckets, connections, number items, total bytes transferred.
* Determine errors in clusters: enjections, out of memory errors and error queries.
* Gain insights into information about the workload of the cluster: percent of used memory, percent of used CPU.

<img src={useBaseUrl('img/integrations/databases/Couchbase-Overview.png')} alt="Cassandra dashboards" />

### Bucket I/O

The **Couchbase - Bucket I/O** dashboard provides an insight into the operators of buckets in clusters: the number of getting operations, the number of set operations, the number of delete operations, the bytes read/write.

Use this dashboard to:
* Get insights into information about the total amount of operations in buckets per second;  the number of delete misses operations, get operations, set operations, update operations in buckets per second.
* Get insights into information about the number of bytes read, bytes written over time.

<img src={useBaseUrl('img/integrations/databases/Couchbase-Bucket-I-O.png')} alt="Cassandra dashboards" />

### Cluster Resources

The **Couchbase - Cluster Resources** dashboard provides an insight into the resources of clusters: the memory resource usage, the CPU resource usage, the disk resource usage.

Use this dashboard to:
* Gain insights into the workload of Couchbase clusters such as the percent of CPU used, the percent of Memory used, the High Low watermark.
* Gain insights into the used resources of Couchbase clusters such as the Disk usage, the Swap space usage, the Memory available.
* Gain insights into the rate requests, rate of streaming requests on the management port.

<img src={useBaseUrl('img/integrations/databases/Couchbase-Cluster-Resources.png')} alt="Cassandra dashboards" />

### DCP Queues

The **Couchbase - DCP Queues** dashboard provides an insight into the DCP queues of buckets in couchbase clusters: the number of DCP connections, DCP senders, the number of items in DCP Queues.

Use this dashboard to:
* Gain insights into the operations of DCP queues. This helps you identify the performance of your clusters when your cluster rebalance

<img src={useBaseUrl('img/integrations/databases/Couchbase-DCP-Queues.png')} alt="Cassandra dashboards" />

### Disk Queues

The **Couchbase - Disk Queues** dashboard provides an insight into the DCP queues of buckets in couchbase clusters: the number of active items waiting to be written to disk, the number of items being put to disk queue, the average age of items in queues.

Use this dashboard to:
* Gain insights into the operations of disk queues. This helps you identify performance about read/write of your clusters.

<img src={useBaseUrl('img/integrations/databases/Couchbase-Disk-Queues.png')} alt="Cassandra dashboards" />

### vBucket

The **Couchbase - vBucket** dashboard provides insights into the state of vBucket of buckets in couchbase clusters: the number of vBucket of buckets, the number items in vBuckets, the state of vBuckets.

Use this dashboard to:
* To determine the number and status of vBucket in your clusters.

<img src={useBaseUrl('img/integrations/databases/Couchbase-vBucket.png')} alt="Cassandra dashboards" />

### XDCR

The **Couchbase - XDCR** dashboard provides insights into replicate operations of buckets cross-cluster: the number of XDCR connections, the number of XDCR items remaining, the number of read-set-delete operations for XDCR.

Use this dashboard to:
* Gain insights into replicate operations of buckets cross-cluster

<img src={useBaseUrl('img/integrations/databases/Couchbase-XDCR.png')} alt="Cassandra dashboards" />

### Errors

The **Couchbase -  Errors** dashboard provides insights into errors from error logs in couchbase servers and couchbase clusters: buckets not ready, nodes not responding, node down, error queries, last error logs.

Use this dashboard to:
* Quickly identify critical errors affecting your couchbase servers.
* Identify SQL error queries from clients.

<img src={useBaseUrl('img/integrations/databases/Couchbase-Errors.png')} alt="Cassandra dashboards" />

### Events

The **Couchbase -  Events** dashboard provides insights into events from couchbase servers and couchbase clusters: the number of login failure, login success from clients, add/remove node events, add/remove bucket events, rebalance events.

Use this dashboard to:
* To audit the activities happening in the cluster. This helps to determine what activities have occurred in the system, helping to control system security.

<img src={useBaseUrl('img/integrations/databases/Couchbase-Events.png')} alt="Couchbase dashboards" />

### HTTP Access

The **Couchbase -  HTTP Access** dashboard provides insights into HTTP Rest API requests from clients to couchbase servers and couchbase clusters: the latency, HTTP codes, client agents, IP clients, errors with 4XX 5XX response code.

Use this dashboard to:
* To understand user behavior accessing clusters and servers through Rest API.

<img src={useBaseUrl('img/integrations/databases/Couchbase-HTTP-Access.png')} alt="Couchbase dashboards" />

## Create monitors for Couchbase app

import CreateMonitors from '../../reuse/apps/create-monitors.md';

<CreateMonitors/>

### Couchbase alerts

<table>
  <tr>
   <td>Alert Type (Metrics/Logs) </td>
   <td>Alert Name </td>
   <td>Alert Description   </td>
   <td>Trigger Type (Critical / Warning) </td>
   <td>Alert Condition   </td>
   <td>Recover Condition   </td>
  </tr>
  <tr>
   <td>Logs   </td>
   <td>Couchbase - Bucket Not Ready   </td>
   <td>This alert fires when a bucket in the Couchbase cluster is not ready.   </td>
   <td>Critical   </td>
   <td> &#60; 0   </td>
   <td>&#60; &#61;0   </td>
  </tr>
  <tr>
   <td>Logs   </td>
   <td>Couchbase - Node Down   </td>
   <td>This alert fires when a node in the Couchbase cluster is down.   </td>
   <td>Critical   </td>
   <td>&#62;   </td>
   <td>&#60; &#61;0   </td>
  </tr>
  <tr>
   <td>Logs   </td>
   <td>Couchbase - Node Not Respond   </td>
   <td>This alert fires when a node in the Couchbase cluster does not respond too many times.   </td>
   <td>Critical   </td>
   <td>&#62; &#61; 10 </td>
   <td>&#60; 10   </td>
  </tr>
  <tr>
   <td>Logs   </td>
   <td>Couchbase - Too Many Error Queries on Buckets   </td>
   <td>This alert fires when there are too many errors queries on a bucket in a Couchbase cluster.   </td>
   <td>Critical   </td>
   <td>&#62; &#61; 1000   </td>
   <td>&#60; 1000   </td>
  </tr>
  <tr>
   <td>Logs   </td>
   <td>Couchbase - Too Many Login Failures   </td>
   <td>This alert fires when there are too many login failures to a node in a Couchbase cluster. </td>
   <td>Critical   </td>
   <td>&#62; &#61; 1000   </td>
   <td>&#60; 1000   </td>
  </tr>
  <tr>
   <td>Metrics   </td>
   <td>Couchbase - High CPU Usage   </td>
   <td>This alert fires when CPU usage on a node in a Couchbase cluster is high.  </td>
   <td>Critical   </td>
   <td>&#62; &#61; 80   </td>
   <td>&#60; 80   </td>
  </tr>
  <tr>
   <td>Metrics   </td>
   <td>Couchbase - High Memory Usage   </td>
   <td>This alert fires when memory usage on a node in a Couchbase cluster is high.  </td>
   <td>Critical   </td>
   <td>&#62; &#61; 80   </td>
   <td>&#60; 80 </td>
  </tr>
</table>
