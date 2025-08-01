---
title: Palo Alto Networks NGFW
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/palo-alto-networks-ngfw.png')} alt="palo-alto-networks-ngfw" width="100"/>

***Version: 1.4
Updated: May 29, 2024***

Issue containment actions during incident investigation through Palo Alto NGFW.

## Actions

* **List Applications** (*Enrichment*) - List all applications.
* **Block IP** (*Containment*) - Block traffic in the specified direction to/from the specified IP.
* **Unblock IP** (*Containment*) - Remove block in the specified direction to/from the specified IP.
* **Block URL** (*Containment*) - Block traffic to/from the specified URL.
* **Unblock URL** (*Containment*) - Remove block to/from the specified URL.
* **Block Port** (*Containment*) - Block traffic from the specified protocol to/from the port.
* **Unblock Port** (*Containment*) - Remove block on the specified protocol to/from the port.
* **Block Application** (*Containment*) - Block traffic to/from specified application.
* **Unblock Application** (*Containment*) - Remove block on specified application.

## External Libraries

* [xmltodict](https://github.com/martinblech/xmltodict)

## Configure Palo Alto Networks NGFW in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **URL**. Enter your Palo Alto Networks NGFW URL.

* **API Key**. Enter a Palo Alto Networks NGFW [API key](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/firewall-administration/manage-firewall-administrators/configure-administrative-accounts-and-authentication/configure-api-key-lifetime#:~:text=After%20you%20specify%20the%20key,the%20API%20Keys%20Last%20Expired.).
* <IntegrationCertificate/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/palo-alto-networks-ngfw-configuration.png')} style={{border:'1px solid gray'}} alt="Palo Alto Networks NGFW configuration" width="400"/>

For information about Palo Alto Networks NGFW, see [Palo Alto Networks Next-Generation Firewall documentation](https://docs.paloaltonetworks.com/ngfw).

## Change Log

* January 14, 2020 - First upload
* July 11, 2023 (v1.3)
	+ Updated the integration with Environmental Variables
	+ Integration renamed from Palo Alto Networks NGFW OIF to Palo Alto Networks NGFW
* May 29, 2024 (v1.4) - Docker updated