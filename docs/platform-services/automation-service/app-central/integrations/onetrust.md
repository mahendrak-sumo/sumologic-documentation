---
title: OneTrust
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/onetrust.png')} alt="onetrust" width="100"/>

***Version: 1.1  
Updated: Feb 5, 2024***

OneTrust is a technology platform that helps organizations comply with privacy and security regulations like GDPR and CCPA by automating privacy assessments, data mapping, and consent management.

## Actions

* **Activate User** *(Containment)* - Activate a specific user.
* **Add Group Member** *(Containment)* - Add a specific user to the given group.
* **Create Incident** *(Notification)* - Create an incident in a specific organization.
* **Create Organization** *(Containment)* - Create a new organization.
* **Deactivate User** *(Containment)* - Deactivate a specific user.
* **Get Incident Details** *(Enrichment)* - Retrieve a specific incident details.
* **List Groups** *(Enrichment)* - Get a list of user groups.
* **List Organizations** *(Enrichment)* - Retrieve the list of all organizations in your tenant.
* **List Users** *(Enrichment)* - Retrieve a paginated list of user records from the OneTrust application.
* **Remove Group Member** *(Containment)* - Remove a specific member from the given group
* **Search Incidents** *(Enrichment)* - Search incidents based on criteria.

## Configure OneTrust in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **URL**. Enter your OneTrust [environment URL](https://my.onetrust.com/s/article/UUID-9abee0a7-3417-4553-f7fb-f2a0c2fc6b2a?language=en_US) in the format `https://<hostname>`, for example `https://app-eu.onetrust.com`

* **API Key**. Enter a OneTrust [API key](https://my.onetrust.com/s/article/UUID-76f55697-ba16-d000-849a-d33e3d217f41?language=en_US).
* <IntegrationTimeout/>
* <IntegrationCertificate/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/onetrust-configuration.png')} style={{border:'1px solid gray'}} alt="OneTrust configuration" width="400"/>

For information about OneTrust, see [OneTrust documentation](https://developer.onetrust.com/).

## Change Log

* January 19, 2024 - First upload
* February 5, 2024 (v1.1) - New action: Create Organization
