---
title: Fidelis Elevate Network
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/fidelis-elevate-network.png')} alt="fidelis-elevate-network" width="80"/>

***Version: 1.1  
Updated: Jul 06, 2023***

Search alerts and retrieve analysis details from Fidelis Network Elevate.

## Actions

* **Retrieve Alert Details** (*Enrichment*) - Retrieve the alert details for the specified alert ID.
* **Retrieve Analytic Info** (*Enrichment*) - Retrieve the analytics for the specified alert ID.
* **Retrieve Endpoint Info** (*Enrichment*) - Retrieve the host details for the specified alert ID.
* **Retrieve Execution Forensics** (*Enrichment*) - Retrieve the execution forensics details for the specified alert ID.
* **Retrieve Malware Info** (*Enrichment*) - Retrieve the malware details for the specified alert ID.
* **Retrieve Session Info** (*Enrichment*) - Retrieve the session details for the specified alert ID.
* **Search Into Alerts** (*Enrichment*) - Search alerts based on the specified search filter.

## Configure Fidelis Elevate Network in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **URL**. Enter the URL of your Fidelis instance.

* **User Name**. Enter the username of a Fidelis admin user authorized to authenticate the integration.

* **Password**. Enter the password for the admin user.
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/fidelis-configuration.png')} style={{border:'1px solid gray'}} alt="Fidelis Elevate configuration" width="400"/>

For information about Fidelis Elevate Network, see [Fidelis documentation](https://fidelissecurity.com/resources/how-tos/).

## Change Log

* June 3, 2019 - First upload
* July 6, 2023 (v1.1) - Updated the integration with Environmental Variables
