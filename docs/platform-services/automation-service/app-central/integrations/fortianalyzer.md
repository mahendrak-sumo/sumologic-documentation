---
title: FortiAnalyzer
description: ''
---

import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/fortianalyzer.png')} alt="fortianalyzer" width="100"/>

***Version: 1.5  
Updated: Mar 4, 2024***

Search events and network traffic from Fortinet FortiAnalyzer.

## Actions

* **Search Into Events** (*Enrichment*) - Search FortiAnalyzer based on the specified criteria.
* **Get Alert Events** (*Enrichment*) - Get alerts based on specific event criteria.
* **Get Alert Event Logs** (*Enrichment*) - Get event logs based on the specified criteria.
* **Search Network Traffic** (*Enrichment*) - Search network traffic based on specific criteria.
* **List Incidents** (*Enrichment*) - List previously generated incidents.
* **Create Incident** (*Notification*) - Create new incident.
* **Update Incident** (*Notification*) - Update a previously created incident.
* **Get Alerts Events Daemon** (*Daemon*) - Daemon to pull FortiAnalyzer Alert Events.
* **Get Alert Events Daemon V2** *(Daemon*) - Daemon to pull FortiAnalyzer Alert Events.

## Configure FortiAnalyzer in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **API URL**. Enter your FortiAnalyzer API URL, for example, `https://192.168.0.10/jsonrpc`

* **Username**. Enter the username of a FortiAnalyzer admin user authorized to authenticate the integration.

* **Password**. Enter the password for the admin user.

* **ADOM Name (Daemon)**. Enter your FortiAnalyzer [ADOM name](https://docs.fortinet.com/document/fortianalyzer/7.6.0/administration-guide/718923/root-adom).

* <IntegrationTimeout/>
* <IntegrationCertificate/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/fortianalyzer-configuration.png')} style={{border:'1px solid gray'}} alt="FortiAnalyzer configuration" width="400"/>

For information about FortiAnalyzer, see [FortiAnalyzer documentation](https://docs.fortinet.com/product/fortianalyzer/7.6).

## Change Log

* June 19, 2019 - First upload
* May 29, 2020 - New action added
* July 21, 2023 (v1.2) - Updated the integration with Environmental Variables
* September 4, 2023 (v1.3) - Fixed a bug where if the timeout was not specified, an error would occur
* September 19, 2023 (v1.4) - Versioning
* March 4, 2024 (v1.5) - Updated code for compatibility with Python 3.12
