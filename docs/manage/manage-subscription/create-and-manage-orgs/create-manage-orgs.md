---
id: create-manage-orgs
title: Create and Manage Orgs
sidebar_label: Credits Accounts
description: Learn how to create and manage multiple Sumo Logic orgs.
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import Iframe from 'react-iframe';

:::note
If you are a Sumo Logic Service Provider, see [Create and Manage Orgs (Service Providers)](create-manage-orgs-service-providers.md).
:::

## Availability

| Account Type | Account Level |
|:--------------|:--------------------------------------------------------------|
| Credits | Enterprise Operations, Enterprise Security, Enterprise Suite |

:::note
This feature is not enabled by default. If you’d like to have it enabled, contact your Sumo Logic Account Executive.
:::

This topic has information about Sumo Logic’s Organizations (“Sumo Orgs”) feature, which you can use to create and manage orgs. We use the term *parent org* to refer to the org from which you create a new org, and *child orgs* to refer to the orgs you create. 

Sumo Orgs allows you to logically group, provision, and centrally manage and monitor the credits usage of multiple orgs.

When you create a child org, you provision it with credits, based on the ingest volume you estimate for the org. We refer to the different flavors of ingest—Continuous Log Ingest, Frequent Log Ingest, and so on—as *product variables*. When you provision a child org you use a Credits Calculator to estimate and allocate required credits for each product variable. 

We refer to your estimates of ingest capacity required for each product variable as *baselines*. Sumo Logic’s throttling multipliers for logs and metrics are based on these estimates. For example, if you estimate 1GB usage for logs and specify that as the baseline when you create the org, Sumo Logic will start [throttling](/docs/manage/ingestion-volume/log-ingestion.md) when ingestion to the org reaches 4 to 10 times the baseline. The multiplier depends on your account size.

Users that have the required role capabilities (described in the following section) can create child orgs under a parent org, and manage and monitor the allocation and consumption of Sumo Logic credits across orgs, and for each child org. This functionality is available in the Sumo Logic UI in the **Organizations** tab and also in the [Organizations Management API](https://organizations.sumologic.com/docs/).

## Create a new child organization

This section has instructions for creating a new org.

:::sumo Micro Lesson
Learn how to create a new child organization.

<Iframe url="https://fast.wistia.net/embed/iframe/hdr7wf9hin?web_component=true&seo=true&videoFoam=false"
  width="854px"
  height="480px"
  title="Micro Lesson: Creating a New Child Org Video"
  id="wistiaVideo"
  className="video-container"
  display="initial"
  position="relative"
  allow="autoplay; fullscreen"
  allowfullscreen
/>

:::

:::note
After you create a new org, you can’t delete it.
:::

1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu, select **Administration > Organizations**.<br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Organizations**. You can also click the **Go To...** menu at the top of the screen and select **Organizations**.
1. Click **+ Add Organization**.<br/> <img src={useBaseUrl('/img/manage/subscriptions/add-org.png')} alt="add-org" style={{border:'1px solid gray'}} width="800" />
1. The **Create New Organization** pane appears. <br/> <img src={useBaseUrl('/img/manage/subscriptions/create-new-org.png')} alt="create-new-org.png" style={{border:'1px solid gray'}} width="400" />

### Allocate Credits

1. **Plan Type**. Select your organization's plan type. 
1. **Deployment**. Select a Sumo Logic deployment from the list.
      :::note
      Depending on your Sumo Logic subscription type, creating an org in a different deployment than the parent may result in a deployment charge being applied on the usage.
      :::
1. Click **Set Baseline**.
      :::note
      You need to set the baseline to allocate credits.
      :::
      <img src={useBaseUrl('img/manage/subscriptions/allocate-credits-set-baseline.png')} alt="allocate-credits-set-baseline" style={{border:'1px solid gray'}} width="450" />
1. In the credits calculator, enter the following:
      * **Continuous Log Ingest**. Enter estimated daily ingestion to the Continuous Tier.
      * **Frequent Log Ingest.** Enter estimated daily ingestion to the Frequent Tier.
      * **Infrequent Log Ingest.** Enter estimated daily ingestion to the Infrequent Tier.
      * **Metrics**. Enter estimated daily metric data points per minute (DPM) ingestion.
      * **Tracing**. Enter estimated daily ingestion of traces.<br/> <img src={useBaseUrl('img/manage/subscriptions/credits-calculator.png')} alt="calculator" style={{border:'1px solid gray'}} width="450" />
1. **Cloud SIEM Enterprise**. Click the checkbox to enable Cloud SIEM. When the **Cloud Log Ingest** field appears, enter a value in GB.
          :::note
          Provisioning Cloud SIEM can take up to 24 hours. See [Monitor Cloud SIEM Provisioning](#monitor-cloud-siem-provisioning), below.
          :::
1. As you enter the ingestion estimates, the number of credits required for the specified ingestion levels will be incremented.
1. The calculator now shows the recommended credit allocation, which provides you a suggestion on how many credits you would need for the child org. This is calculated based on the baseline added, the burndowns in your contract, and the days remaining in your contract.
1. Throttling limits displays the rate of ingestion. To learn more, refer to [Log Ingestion](/docs/manage/ingestion-volume/log-ingestion/).
1. Once you're finished adding the baseline, click **Done**.
1. **Credits to be allocated**. The recommended credits for this child org will be displayed once you set the baseline. You can add or reduce the credit based on your requirement.
1. **Remaining Credits (Parent)**. Total balance credits available in the parent org after using the credits for child org.

### Basic Details

1. **Organization Name**. Enter a unique name for the org.
1. **Account Owner Email**. Enter the name of the account owner.
1. **Account Owner First Name**. Enter the account owner’s first name.
1. **Account Owner Last Name**. Enter the account owner’s last name.<br/> <img src={useBaseUrl('img/manage/subscriptions/basic-details.png')} alt="basic-details" style={{border:'1px solid gray'}} width="450" />

## Update an org's credits allocation

:::sumo Micro Lesson
Watch this micro lesson to learn more about managing a child org.

<Iframe url="https://fast.wistia.net/embed/iframe/cijbsnbz65?web_component=true&seo=true&videoFoam=false"
  width="854px"
  height="480px"
  title="Micro Lesson: Managing Orgs with Flexible Credits Allocation Video"
  id="wistiaVideo"
  className="video-container"
  display="initial"
  position="relative"
  allow="autoplay; fullscreen"
  allowfullscreen
/>

:::

:::info
If the org has already depleted its credits, you can see a red color warning icon in the **Usage %** column and a red-colored usage bar in the **Allocation & usage** section when you view the selected org.
:::

To change an org's credits allocation:
1.  [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu, select **Administration > Organizations**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Organizations**. You can also click the **Go To...** menu at the top of the screen and select **Organizations**.
1. Click the row for the org you want to edit.
1. Click **Edit** in the right hand pane. <br/> <img src={useBaseUrl('img/manage/subscriptions/edit-org-3.png')} style={{border:'1px solid gray'}} alt="your description" width="450" />
1. Under **Modify Allocation**, you can increase or decrease the credits based on your requirement.<br/> <img src={useBaseUrl('img/manage/subscriptions/modify-allocation-button.png')} style={{border:'1px solid grey'}} alt="allocation" width="450" />
      :::note
      Ensure your total new allocation is not less than the consumed credits.
      :::
      You can select **Credits to be Added** and enter the recommended credit value or value of your choice to top-up additional credits to avoid credit depletion. This recommendation is calculated based on the usage forecast and average credit usage per day for the selected contract period.

      **Example 1**: Consider that you are heading towards credit depletion and need to add more credits. The below image shows you have almost run out of the credits (1840/1841) allocated to the child org. To increase the credits to the child org, select **Credits to be Added**. An info icon will appear below with a suggestion on how many credits can be added to this child org based on the usage forecast. So, in this case, it is suggested that you add 135 credits.<br/> <img src={useBaseUrl('img/manage/subscriptions/modify-allocation-recommendation.png')} style={{border:'1px solid gray'}} alt="modify-allocation-recommendation" width="450" />
      <br/>Click **View Details** to view the detailed breakdown of the recommended value.<br/> <img src={useBaseUrl('img/manage/subscriptions/modify-allocation-usage-forecast.png')} style={{border:'1px solid gray'}} alt="modify-allocation-usage-forecast" width="450" />

      **Example 2**: Suppose you need to increase credits for your child org. The image below shows that you have used 35 out of 31,026 credits allocated to your child org. To add more credits, select **Credits to be Added** and enter the additional credits required.<br/> <img src={useBaseUrl('img/manage/subscriptions/modify-allocation-recommendatio-example-2.png')} style={{border:'1px solid gray'}} alt="modify-allocation-recommendatio-example-2" width="450" />
1. If you want to modify the baseline, click **View Baseline**. The credits calculator appears.
      1. Click **Edit** and follow the steps in [Allocate Credits](#allocate-credits) to update the credits allocation.<br/> <img src={useBaseUrl('img/manage/subscriptions/edit-baseline.png')} style={{border:'1px solid gray'}} alt="edit-baseline" width="450" />
      1. Once you save the new baseline, you can view the recommended value in the **Credits to be Added/Reduced** section.<br/><img src={useBaseUrl('img/manage/subscriptions/baseline-credits-to-add.png')} style={{border:'1px solid gray'}} alt="baseline-credits-to-add" width="450" />
      1. (Optional) Click **View Details** to view the detailed breakdown of additional credits required value.<br/><img src={useBaseUrl('img/manage/subscriptions/baseline-view-details.png')} style={{border:'1px solid gray'}} alt="baseline-view-details" width="450" />
1. Click **Save** once you finish editing the credit values.

## Monitor Cloud SIEM provisioning

You can also enable and provision Cloud SIEM for a child org. Note that the process of provisioning Cloud SIEM can take up to 24 hours. You can determine provisioning status on the **Organizations** page. Until the provisioning is complete, you'll see a spinner and message that indicates the process is on-going.

<img src={useBaseUrl('img/cse/status.png')} style={{border:'1px solid gray'}} alt="status" />

## Monitor credits allocation and usage

This section has information about how you can monitor credit allocations and consumption across all your orgs, and for each child org. You can view the usage by orgs and child orgs on the **Account Overview** page.

* The **Credit Allocation** tile indicates the total number of credits allocated.
* The **Total Credit Usage** tile shows amount of credits consumed out of the total allocated credits.
* The **Usage Forecast** tile indicates the total consumption of the credits by the end of contract period according to the current consumption rate.

### View overall credit usage by category

You can view the aggreagte usage for all child orgs across usage category in the **Overall Usage** tab on the **Account Overview** page. Usage for the following categories is shown:

* **Continuous Ingest**. Credits used for logs ingested into the Continuous Tier.
* **Frequent Ingest**. Credits used for logs ingested into the Frequent Tier.
* **Infrequent Ingest**. Credits used for logs ingested into the Infrequent Tier.
* **Infrequent Scan**. Credits used to scan data for Infrequent Tier searches.
* **Tracing Ingest**. Credits used for traces ingested.
* **Metrics Ingest**. Credits used for metrics ingested.
* **Storage**. Credits used for log storage in the Continuous and Frequent Tiers.
* **Cloud SIEM Ingest**. Credits used for logs ingested into Cloud SIEM.
* **Infrequent Storage**. Credits used for log storage in the Infrequent Tier.
* **Promotional categories**. For more information, see [Promotional Credits](/docs/manage/manage-subscription/sumo-logic-credits-accounts/#promotional-credits).

By clicking the **Download Report** button, you can download the org usage data in csv format for further analysis and reporting. You can also download the detailed child org usage data in csv format by clicking **Download Detailed Credit Usages** from the kebab icon next to the Download Report button.

<img src={useBaseUrl('img/manage/subscriptions/account-overview-new.png')} alt="your description" style={{border:'1px solid gray'}} />

### View child credit usage by category

You can view the usage of individual child orgs in the **Child Orgs** tab on the **Account Overview** page. Usage for the following categories is shown:

* The **Allocated Credits** column indicates the total number of credits allocated to the selected child org.
* The **Total Credits Used** column indicates the total amount of credits consumed for the selected child org.
* The **Usage %** column indicates the percentage of overall subscription credits that was allocated to a child org has been consumed.
* The **Forecast** column indicates the percentage of total consumption of the credits by the end of contract period according to the current consumption rate for a selected child org.
* The **Usage % Change** column indicates the difference in credit usage by your child organisations over time. For example, if you choose the date from Jan 1 to Jan 7, it will show the usage difference between Dec 25 to Dec 31 and Jan 1 to Jan 7 for each child organisation.

By clicking the **Download Report** button, you can download the child org usage data in csv format for further analysis and reporting.

<img src={useBaseUrl('img/manage/subscriptions/child-org.png')} alt="your description" style={{border:'1px solid gray'}} />

By clicking any of the selected child orgs, a side panel opens up in which you can view the individual usage for the following categories:

* **Continuous Ingest**. Credits used for logs ingested into the Continuous Tier for the selected child org.
* **Frequent Ingest**. Credits used for logs ingested into the Frequent Tier for the selected child org.
* **Infrequent Ingest**. Credits used for logs ingested into the Infrequent Tier for the selected child org.
* **Infrequent Scan**. Credits used to scan data for Infrequent Tier searches.
* **Tracing Ingest**. Credits used for traces ingested for the selected child org.
* **Metrics Ingest**. Credits used for metrics ingested for the selected child org.
* **Storage**. Credits used for log storage in the Continuous and Frequent Tiers for the selected child org.
* **Cloud SIEM Ingest**. Credits used for logs ingested into Cloud SIEM for the selected child org.
* **Infrequent Storage**. Credits used for log storage in the Infrequent Tier for the selected child org.

By clicking the **Download Report** button, you can download the selected child org usage data in csv format for further analysis and reporting.

<img src={useBaseUrl('img/manage/subscriptions/child-credit-usage.png')} alt="your description" style={{border:'1px solid gray'}} />

## Audit logging for organizations

This section has examples of the messages Sumo Logic writes to the Audit Event Index when you create or update an org.

### OrganizationCreated

```
{"operator":{"email":"PF6gIXhGj33LzP8Ba7kzJkL9EAtz6RuSMYHHKnqbEICUz7PITMdceGFRdQESZ2JwpL7BSbGaOaUYmgADz3USgExa7egWZ8UY2wmm","id":"000000000AEE3701","interface":"INTERNAL","sessionId":"no_session","sourceIp":"54.177.241.252","type":"UserContext"},"organizationIdentity":{"email":"pjain+1ljjjjjlwasdk@sumologic.com","organizationName":"akjdhfjkhwqiue","firstName":"kajsdfh","lastName":"sakjdsh","orgId":"000000000AF45F28","deploymentId":"nite"},"subscriptionDetails":{"startDate":{"year":2020,"month":12,"day":16},"endDate":{"year":2021,"month":5,"day":11},"plan":"SUMO-ENT-SUI","credits":4388,"baselines":{"continuousIngest":1,"continuousStorage":30,"frequentIngest":1,"frequentStorage":30,"infrequentIngest":1,"infrequentStorage":30,"infrequentScan":0,"metrics":1,"cseIngest":0,"cseStorage":0}},"eventType":"Audit","severityLevel":"Info","accountId":"000000000AE07391","eventId":"67718e94-a8a3-4478-b778-975cd0e18e55","eventName":"OrganizationCreated","eventTime":"2020-12-17T04:59:05.584Z","eventFormatVersion":"1.0 beta","subsystem":"organizations"}
```

### OrganizationUpdated

```
{"operator":{"email":"rmVOQ8CwUBuxqDydSLIV2ZljEogPcR7fwZiIShCBldZ5jhEl1c4wLHJhL1j6xkzVPVuqVJFf1c5VUvOMESv4ieY0xrxuVET6JXTw","id":"000000000B0858AE","interface":"INTERNAL","sessionId":"no_session","sourceIp":"122.181.102.148","type":"UserContext"},"organizationIdentity":{"email":"fkonkpjg3z@mail.peaksum.com","organizationName":"QEV9Z3Z","firstName":"ne3ieXF","lastName":"85lZdXD","orgId":"000000000AF35705","deploymentId":"nite"},"from":{"continuousIngest":200,"continuousStorage":30,"frequentIngest":200,"frequentStorage":30,"infrequentIngest":200,"infrequentStorage":30,"infrequentScan":0,"metrics":200,"cseIngest":0,"cseStorage":0},"to":{"continuousIngest":201,"continuousStorage":30,"frequentIngest":200,"frequentStorage":30,"infrequentIngest":200,"infrequentStorage":30,"infrequentScan":0,"metrics":200,"cseIngest":0,"cseStorage":0},"eventType":"Audit","severityLevel":"Info","accountId":"000000000AE07391","eventId":"2f816b83-3f10-434f-b5b9-e73aa223fd5d","eventName":"OrganizationUpdated","eventTime":"2020-12-16T11:29:53.428Z","eventFormatVersion":"1.0 beta","subsystem":"organizations"}
```
