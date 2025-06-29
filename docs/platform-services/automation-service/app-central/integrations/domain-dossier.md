---
title: Domain Dossier
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/domain-dossier.png')} alt="domain-dossier" width="100"/>

***Version: 1.1  
Updated: Jul 06, 2023***

Perform WHOIS queries with Domain Dossier.

## Actions

* **Whois** (*Enrichment*) - Get WHOIS information for an IP or domain.

## Configure Domain Dossier in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **Endpoint**. Enter a Domain Dossier endpoint address.

* **Username**. Enter the username of a Domain Dossier admin user authorized to provide authentication for the integration. 

* **Password**. Enter the password for the admin user.
* <IntegrationTimeout/>
* <IntegrationCertificate/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/domain-dossier-configuration.png')} style={{border:'1px solid gray'}} alt="Domain Dossier configuration" width="400"/>

For information about Domain Dossier, see the [Domain Dossier website](https://centralops.net/co/domaindossier).

## Change Log

* May 7, 2019 - First upload
* July 6, 2023 (v1.1) - Updated the integration with Environmental Variables
