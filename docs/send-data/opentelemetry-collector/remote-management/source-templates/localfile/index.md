---
slug: /send-data/opentelemetry-collector/remote-management/source-templates/localfile
title: Local File Source Template
sidebar_label: Local File
description: Learn about the Sumo Logic Local File source template for OpenTelemetry.
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<img src={useBaseUrl('img/send-data/otel-color.svg')} alt="Thumbnail icon" width="30"/>

The Local File source template generates an OpenTelemetry configuration that can be sent to a remotely managed OpenTelemetry collector (abbreviated as otelcol). By creating this source template and deploying the configuration to the appropriate OpenTelemetry agent, you can ensure your logs are collected and sent to Sumo Logic.

## Fields created by the source template

When you create a source template, the following [fields](/docs/manage/fields/) are automatically added (if they don’t already exist):

- **`sumo.datasource`**. Fixed value of **localfile**.
- **`deployment.environment`**. User configured field at the time of collector installation. This identifies the environment where the host resides. For example: `dev`, `prod`, or `qa`.
- **`host.group`**. This is a collector-level field that is user configured at the time of collector installation. It identifies the host group from where logs are collected.
- **`host.name`**. This is tagged through the resourcedetection processor. It holds the value of the host name where the OTel collector is installed.

## Prerequisites

### For logs collection
import LogsCollectionPrereqisites from '../../../../../reuse/apps/logs-collection-prereqisites.md';

<LogsCollectionPrereqisites/>

import OtelWindowsLogPrereq from '../../../../../reuse/apps/opentelemetry/log-collection-prerequisite-windows.md';

<OtelWindowsLogPrereq/>

## Configuring the Local File source template

Follow these steps to set up and deploy the source template to a remotely managed OpenTelemetry collector.

### Step 1: Set up remotely managed OpenTelemetry collector

import CollectorInstallation from '../../../../../reuse/apps/opentelemetry/collector-installation.md';

<CollectorInstallation/>

### Step 2: Configure the source template

In this step, you will configure the yaml required for Local File collection. Below are the inputs required for configuration:

- **Name**. Name of the source template.
- **Description**. Description for the source template.
- **Fields/Metadata**. You can provide any customer fields to be tagged with the data collected. By default, sumo tags `_sourceCategory` with the value otel/localfile.
- **File Path**. Provide the file which needs to be read by OpenTelemetry agent. You can provide path to multiple files by adding new entry to it.
- **DenyList**. Provide path expression describing the files to be excluded.
- **Collection should begin from**. Defines where will the collection of the logs start from. Possible values are "End of File" and "Beginning of File".
- **Detect messages spanning multiple lines**. You can enable this option when dealing with logs which span over multiple lines. On enabling this option you will need to specify **Boundary regex location** where you can specify if the expression defines end or start of the log line and **Expression to match message boundary** where you will define the expression.

import TimestampParsing from '../../../../../reuse/apps/opentelemetry/timestamp-parsing.md';

<TimestampParsing/>

**Processing Rules**. You can add processing rules for logs collected. To learn more, refer to [Processing Rules](../../processing-rules/index.md).

### Step 3: Push the source template to the desired remotely managed collectors

import DataConfiguration from '../../../../../reuse/apps/opentelemetry/data-configuration.md';

<DataConfiguration/>

:::info
Refer to the [changelog](changelog.md) for information on periodic updates to this source template.
:::
