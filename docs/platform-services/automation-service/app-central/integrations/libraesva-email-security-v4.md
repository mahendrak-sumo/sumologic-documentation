---
title: Libraesva Email Security V4
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/libraesva-email-security-v4.png')} alt="libraesva-email-security-v4" width="100"/>

***Version: 4.1  
Updated: Jul 11, 2023***

Libraesva Email Security V4 provides security, continuity, and compliance capabilities that include the Email Security Gateway, the Email Load Balancer and the Email Archiver.

## Actions

* **Get Black List** (*Enrichment*) - Return a list of blacklist for a mailbox.
* **Get Mail Log** (*Enrichment*) - View the emails of a user for a given day (ISO notation Y / m / d or Ymd).
* **Get Spam Log** (*Enrichment*) - Display a user’s email for a given day (ISO notation Y / m / d or Ymd).
* **Get Valid Recipients** (*Enrichment*) - Get All Address in Valid Recipient.
* **Get Recipients Exist** (*Enrichment*) - Check if an address belongs to a Valid Recipient.
* **Get User List** (*Enrichment*) - Show the list of users with attributes.
* **Get White List** (*Enrichment*) - Return the list of whitelisted for a mailbox, code 802 if the list is empty.

## Category

Email Security

## Configure Libraesva Email Security in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **URL**. Enter your Libraesva URL.

* <IntegrationTimeout/>
* <IntegrationCertificate/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/libraesva-v4-configuration.png')} style={{border:'1px solid gray'}} alt="Libraesa V4 configuration" width="400"/>

For information about Libraesva Email Security V4, see [Libraesva Email Security V4 documentation](https://docs.libraesva.com/doc/libraesva-esg-4/).

## Change Log

* May 11, 2021 - First upload
* September 12, 2022 - Changed integration name and logo
* July 11, 2023 (v4.1) - Updated the integration with Environmental Variables
