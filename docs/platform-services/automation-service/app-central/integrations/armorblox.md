---
title: Armorblox
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/armorblox.png')} alt="armorblox" width="70"/>

***Version: 1.1  
Updated: Sep 04, 2023***

Armorblox secures enterprise communications over email and other cloud office applications with the power of Natural Language Understanding. The Armorblox platform connects over APIs and analyzes thousands of signals to understand the context of communications and protect people and data from compromise. Armorblox connects over APIs with Office 365, G Suite, and Exchange to secure your human layer without affecting MX records or mail flow. Tens of thousands of organizations use Armorblox to stop BEC and targeted phishing attacks, protect sensitive PII and PCI, and automate remediation of user-reported email threats.

## Actions

* **Armorblox Incidents Daemon** *(Daemon)* - Automatically retrieve incidents from Armorblox.
* **Get App Restrictions** *(Enrichment)* - Retrieve info about what actions are available, what each email label's ID is, AD group UIds.
* **Get Incident** *(Enrichment)* - Get information about a specific incident.
* **Get Incident Senders** *(Enrichment)* - Get information about an incident's sender data.
* **List Incidents** *(Enrichment)* - Get a list of all the Incidents detected by Armorblox.
* **Update Incident Action** *(Containment)* - Update the action to be taken for an incident's objects.

## Configure Armorblox in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **Tenant Name**. Enter your [Armorblox tenant name](https://armorblox.stoplight.io/docs/public-apis/6e3583e334a34-armorblox-api#tenant-name).

* **API Key**. Enter the [Armorblox API key](https://armorblox.stoplight.io/docs/public-apis/6e3583e334a34-armorblox-api#api-key).

* **Mock API**. Select to run the integration on a [mock server](https://armorblox.stoplight.io/docs/public-apis/6e3583e334a34-armorblox-api#api-key).
* <IntegrationTimeout/>
* <IntegrationCertificate/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/armorblox/armorblox-configuration.png')} style={{border:'1px solid gray'}} alt="Armorblox configuration" width="400"/>

For information about Armorblox, see [Armorblox documentation](https://armorblox.stoplight.io/docs/public-apis/6e3583e334a34-armorblox-api).

## Change Log

* September 4, 2023 (v1.0) - First upload
