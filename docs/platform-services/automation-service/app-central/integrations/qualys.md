---
title: Qualys
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/qualys.png')} alt="qualys" width="100"/>

***Version: 1.4  
Updated: Sep 19, 2023***

Launch and manage scans and utilize Qualys scan data to enrich incident artifacts.

## Actions

* **List Scan** (*Enrichment*) - Generates a list of previously executed scans.
* **Launch VM Scan** (*Enrichment*) - Launch a new VM scan.
* **Get Scan Result** (*Enrichment*) - Gather results of an executed scan.
* **Add Asset** (*Enrichment*) - Add an asset.
* **List Asset Group** (*Enrichment*) - Generates a list of all asset groups.
* **List Asset** (*Enrichment*) - Generates a list of all available assets.
* **Get Scanner Details** (*Enrichment*) - Gather details of a specific scanner.
* **List Option Profiles** (*Enrichment*) - Generates a list of all available option profiles.
* **Add Asset Group** (*Enrichment*) - Add an asset group.
* **Launch VM Scan By Tag** (*Enrichment*) - Launch a VM scan.
* **Get Tags** (*Enrichment*) - Get all tags.
* **List Scanner Appliance** (*Enrichment*) - List information on a scanning appliance.
* **List Host Detection** (*Enrichment*) - Gathers QIDs for a host (used with *List Vulnerabilities, see note)*.
* **Assets View Search** (*Enrichment*) - Gathers QIDs for a particular asset (used with *List Vulnerabilities, see note)*.
* **List Vulnerabilities** (*Enrichment*) - Used with *List Host Detection* & *Assets View Search* to gather vulnerabilities of a particular asset (*see notes*).
* **Get Report Templates** (*Enrichment*) - Gathers all report template information.
* **Launch Scan Report** (*Enrichment*) - Launch a new scan report.
* **List Reports** (*Enrichment*) - List all reports.
* **Download Saved Report** (*Enrichment*) - Download a saved report.
* **List Scanned Hosts** (*Enrichment*) - List all scanned hosts.
* **Manage VM Scans** (*Containment*) - Manage existing VM scans.
* **Cancel Report** (*Containment*) - Cancel a report.
* **Delete Report** (*Containment*) - Delete a report.
* **Report Status Polling** (*Containment*) - Gather details about a report's progress.
* **Scan Status Polling** (*Containment*) - Gather details about a scan's status.
* **Tag Existence Polling** (*Containment*) - Tag an existing poll.

## External Libraries

* [xmltodict](https://github.com/martinblech/xmltodict/blob/master/LICENSE)

## Configure Qualys in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';
import IntegrationCertificate from '../../../../reuse/automation-service/integration-certificate.md';
import IntegrationEngine from '../../../../reuse/automation-service/integration-engine.md';
import IntegrationLabel from '../../../../reuse/automation-service/integration-label.md';
import IntegrationProxy from '../../../../reuse/automation-service/integration-proxy.md';
import IntegrationTimeout from '../../../../reuse/automation-service/integration-timeout.md';

<IntegrationsAuth/>
* <IntegrationLabel/>
* **Qualys API Server**. Enter your [Qualys API server URL](https://docs.qualys.com/en/edr/api/#t=getting_started%2Fapi_conventions.htm), for example, `https://qualysapi.qg2.apps.qualys.eu/`.

* **Username**. Enter the username of a Qualys admin user authorized to authenticate the integration.

* **Password**. Enter the password for the admin user.

* **API X-Requested-With**. Enter the "X-Requested-With" header to use with [authentication](https://docs.qualys.com/en/vm/api/scanauth/get_started/authentication.htm).
* <IntegrationTimeout/>
* <IntegrationCertificate/>
* <IntegrationEngine/>
* <IntegrationProxy/>

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/misc/qualys-configuration.png')} style={{border:'1px solid gray'}} alt="Qualys configuration" width="400"/>

For information about Qualys, see [Qualys documentation](https://www.qualys.com/documentation/).

## Change Log

* February 21, 2020 - First upload
* September 2, 2020 - New actions added
* July 21, 2023 (v1.2) - Updated the integration with Environmental Variables
* September 4, 2023 (v1.3) - Fixed a bug where if the timeout was not specified, an error would occur
* September 19, 2023 (v1.4) - Versioning
