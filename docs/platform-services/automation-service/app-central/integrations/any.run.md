---
title: ANY.RUN
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/any.run.png')} alt="any.run" width="80"/>

***Version: 1.4  
Updated: Feb 04, 2025***

Gather detonation data for files and URL using `ANY.RUN`.

## Actions

* **Detonate File** (*Enrichment*) - Detonate a file.
* **Detonate URL** (*Enrichment*) - Detonate a URL.
* **Get Report** (*Enrichment*) - Gather detonation report.

## ANY.RUN configuration

Sign in to ANY.RUN. Click on your profile on the left menu. In the API and Limits tab generate your API KEY and copy it.

## Configure ANY.RUN in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>

* <IntegrationLabel/>
* **URL**. Enter the ANY.RUN URL. The default value is `https://api.any.run`

* **API Key**. Enter the API key you copied [above](#anyrun-configuration).
* <IntegrationCertificate/>
* <IntegrationTimeout/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/anyrun-configuration.png')} style={{border:'1px solid gray'}} alt="ANY.RUN configuration" width="400"/>

For information about ANY.RUN, see [ANY.RUN documentation](https://any.run/cybersecurity-blog/ti-feeds-integration/).
 
## Change Log

* February 21, 2020 - First upload
* February 13, 2023 - Integration refactored
* June 15, 2023 (v1.3) - Updated the integration with Environmental Variables
* February 04, 2025 (v1.4) - Updated the action Get Report with Environmental Variables