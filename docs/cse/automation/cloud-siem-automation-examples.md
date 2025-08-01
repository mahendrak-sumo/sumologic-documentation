---
id: cloud-siem-automation-examples
title: Cloud SIEM Automation Examples
sidebar_label: Automation Examples
description: See examples that show you how to create automations for different situations.   
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import ActionsLimit from '../../reuse/actions-limit.md';

Following are examples that show you how to create Cloud SIEM automations using the [Automation Service](/docs/platform-services/automation-service/). The examples, which are listed in order from simple (performing a basic automation using an out-of-the-box integration) to advanced (creating a custom integration), illustrate many of the tasks you’ll perform on a regular basis when you create your own automations.

:::note
<ActionsLimit/>
:::

## Simple example: Configure an enrichment

The following example shows how to add an enrichment to an insight using the “IP Reputation V3” action from VirusTotal.

1. Edit the VirusTotal OIF resource:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu, select **Automation** and then select **Integrations** in the left nav bar. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Integrations**. You can also click the **Go To...** menu at the top of the screen and select **Integrations**.   
   1. Select **VirusTotal OIF**.
   1. Hover your mouse over the resource name and click the **Edit** button that appears.<br/><img src={useBaseUrl('img/cse/automation-examples-virus-total-resource-edit-button.png')} alt="Resource edit button" style={{border: '1px solid gray'}} width="600"/>
   1. In the **Edit resource** dialog, enter the **API URL**: `https://www.virustotal.com`.
   1. Enter the **API Key**. See the [VirusTotal documentation](https://support.virustotal.com/hc/en-us/articles/115002100149-API) to learn how to obtain the API key. If you do not already have a VirusTotal account, you need to create one to get an API key.
   1. Click **Save**.<br/><img src={useBaseUrl('img/cse/automation-examples-virus-total-edit-resource.png')} alt="Edit resource" style={{border: '1px solid gray'}} width="400"/>
1. Create the playbook:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu, select **Automation > Playbooks**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Playbooks**. You can also click the **Go To...** menu at the top of the screen and select **Playbooks**.
   1. Click the **+** button to the left of **Playbook**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-playbook-button.png')} alt="Add playbook button" width="300"/>
   1. In the **New playbook** dialog, give your playbook a **Name**.
   1. For **Type**, enter **CSE**.
   1. Enter a **Description**.
   1. Click **Create**.
1. Add the “IP Reputation V3” action to the playbook:
   1. Click the **Edit** button (pencil icon) at the bottom of the playbook view.
   1. Click the **Edit** button (pencil icon) on the **START** node.
   1. In the **Edit node** dialog, select **Insight** from the dropdown menu and click **UPDATE**.
   1. Click the **Add Node** button (**+** icon) on the **START** node.
   1. Select **Action**.
   1. In the **Add node** dialog, for **Integration** select **VirusTotal OIF**.
   1. Ensure that **Type** is **Enrichment**.
   1. For **Action**, select **IP reputation V3**.
   1. To the right of the **IPs** field, click the gear icon.
   1. Click [**Playbook inputs**](#playbook-inputs) and select **input.entity.value**.
   1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-ip-reputation-v3-add-node.png')} alt="Add node for IP Reputation V3" style={{border: '1px solid gray'}} width="500"/>
1. Add an enrichment action to the playbook:
   1. Hover your mouse over the **IP reputation V3** node and click the **Add Node** button (**+** icon).
   1. Select **Action**.
   1. In the **Add node** dialog, for **Integration**, select **Sumo Logic Cloud SIEM Internal**.
   1. For **Type**, select **Notification**.
   1. For **Action**, select **Add Insight Enrichment**.
   1. To the right of the **Insight ID** field, click the gear icon.
   1. Click [**Playbook inputs**](#playbook-inputs) and select **input.readableId**.
   1. In the **Enrichment name** field, type **VirusTotal IP reputation**.
   1. To the right of the **Raw JSON** field, click the gear icon.
   1. Click **IP reputation V3** and select **output.raw**.
   1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-insight-enrichment-node.png')} alt="Add node for insight enrichment" style={{border: '1px solid gray'}} width="500"/>
   1. Click and hold on the right semicircle of the new **Add Insight Enrichment** node and drag to the semicircle of the **END** node and release. The playbook is complete.
1. Save the playbook:
   1. Click the **Save** button (floppy disk icon) at the bottom of the playbook view.
   1. To [test the playbook](/docs/platform-services/automation-service/automation-service-playbooks/#test-a-playbook), click the kebab button in the upper-right of the UI and select **Run Test**. 
   1. Click the **Publish** button (clipboard icon) at the bottom of the playbook view. The playbook should look like this:<br/><img src={useBaseUrl('img/cse/configure-an-enrichment-playbook.png')} alt="Simple playbook for insight enrichment" style={{border: '1px solid gray'}} width="700"/>
1. Create an automation in Cloud SIEM to run the playbook:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu select **Cloud SIEM**. In the top menu select **Configuration**, and then under **Integrations** select **Automation**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the top menu select **Configuration**, and then under **Cloud SIEM Integrations** select **Automation**. You can also click the **Go To...** menu at the top of the screen and select **Automation**. 
   1. At the top of the **Automation** tab, click **+ Add Automation**.
   1. For **Playbook**, select the playbook you created in the previous steps.
   1. For **Object (expects attributes for)**, select **Insight**.
   1. For **Execution**, select **Manually Done**.
   1. Click **Save**.
1. Run the automation:
   1. Select **Insights** from the main Cloud SIEM screen.
   1. Select an insight.
   1. Click the Actions button.
   1. Under **Insight Automation**, select the automation you created in the previous step (it will have the same name as the playbook). The playbook runs.
   1. To see the results of the run, click the **Automations** tab at the top of the insight.
   1. View the **Status** field to find out if the playbook has a status of Success or another status such as **Completed with errors**.
   1. Click **View Playbook** to see details of the playbook run. Each node in the playbook will show either **Success** or **Failed**.
   1. Click a node to download results of that node’s run.

### Playbook inputs

Depending on the action, you may need to select a playbook input. The playbook inputs define the kind of input data needed for the action. For descriptions of the playbook inputs, see the responses on the [Get an Insight API](https://api.sumologic.com/docs/sec/#operation/GetInsight).

<img src={useBaseUrl('img/cse/Playbook_inputs.png')} alt="Playbook inputs" style={{border: '1px solid gray'}} width="500"/>

## Intermediate example: Configure a notification

The following example shows how to configure a notification that sends an email upon completion of an action to perform a log search in Sumo Logic core platform.

1. Edit the Sumo Logic resource: 
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu, select **Automation** and then select **Integrations** in the left nav bar. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Integrations**. You can also click the **Go To...** menu at the top of the screen and select **Integrations**. 
   1. Select **Sumo Logic**.
   1. Hover your mouse over the resource name and click the **Edit** button that appears.<br/><img src={useBaseUrl('img/cse/automation-examples-sumo-logic-cip-resource-edit-button.png')} alt="Resource edit button" style={{border: '1px solid gray'}} width="600"/>
   1. In the **Edit  resource** dialog, enter the **API URL** for your Sumo Logic core platform instance (for example, `https://api.us2.sumologic.com`). For the URL to use for your Sumo Logic instance, see [Sumo Logic Endpoints by Deployment and Firewall Security](/docs/api/about-apis/getting-started#sumo-logic-endpoints-by-deployment-and-firewall-security).
   1. [Create an access key](/docs/manage/security/access-keys#create-an-access-key) and copy the resulting access ID and access key.
   1. Enter the **Access ID** and the **Access Key**.
   1. Select your **Time Zone**.
   1. Click **Save**.<br/><img src={useBaseUrl('img/cse/automation-examples-edit-sumo-logic-resource.png')} alt="Edit a resource" style={{border: '1px solid gray'}} width="400"/>
1. Create the playbook:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic).  In the main Sumo Logic menu, select **Automation > Playbooks**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Playbooks**. You can also click the **Go To...** menu at the top of the screen and select **Playbooks**.
   1. Click the **+** button to the left of **Playbook**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-playbook-button.png')} alt="Add playbook button" style={{border: '1px solid gray'}} width="300"/>
   1. In the **New playbook** dialog, give your playbook a **Name**, such as **Notification for a log search**.
   1. For **Type**, enter **CSE**.
   1. Enter a **Description**.
   1. Click **Create**.
1. Add the "Search Sumo Logic" action to the playbook:
   1. Click the **Edit** button (pencil icon) at the bottom of the playbook view.
   1. Click the **Edit** button (pencil icon) on the **START** node.
   1. In the **Edit node** dialog, select **Insight** from the dropdown menu and click **UPDATE**.
   1. Click the **Add Node** button (**+** icon) on **START**.
   1. In the **Add node** dialog, select **Action**.
   1. For **Integration**, select **Sumo Logic**.
   1. Ensure that **Type** is **Enrichment**.
   1. For **Action**, select **Search Sumo Logic**.
   1. In the **Query** box enter the search query you want to make in the Sumo Logic core platform. For help with queries, see [General Search Examples Cheat Sheet](/docs/search/search-cheat-sheets/general-search-examples/).
   1. For **Last Period** select **1 Hour**.
   1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-search-sumo-logic-node.png')} alt="Add Search Sumo Logic node" style={{border: '1px solid gray'}} width="600"/>
1. Add the "Send Email" action to the playbook:
   1. Hover your mouse over the new **Search Sumo Logic** node.
   1. Click the **Add Node** button (**+** icon) at the bottom of the **Search Sumo Logic** node.
   1. Select **Action**.
   1. In the **Add node** dialog, for **Integration** select **Basic Tools**.
   1. Ensure that **Type** is **Notification**.
   1. For **Action** select **Send Email**.
   1. In **Recipients** enter your email address and press Enter.
   1. For **Subject** type a subject line for the email (for example, "Results of Sumo Logic log search").
   1. In **Plain text content** enter the text you want to appear in the body of the email. For example, enter "Search in Sumo Logic was executed. Click the Automations tab at the top of the insight for which the 'Notification for a log search' automation was run. Click 'View Playbook' to see the results."
   1. Copy the plain text content into **HTML content** and add formatting if desired.
   1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-send-email-node.png')} alt="Add Send Email node" style={{border: '1px solid gray'}} width="600"/>  
   1. Click and hold on the right semicircle of the new **Send Email** node and drag to the semicircle of the **END** node and release. The playbook is complete.
1. Save the playbook:
   1. Click the **Save** button (floppy disk icon) at the bottom of the playbook view.
   1. To [test the playbook](/docs/platform-services/automation-service/automation-service-playbooks/#test-a-playbook), click the kebab button in the upper-right of the UI and select **Run Test**. 
   1. Click the **Publish** button (clipboard icon) at the bottom of the playbook view. The playbook should look like this:<br/><img src={useBaseUrl('img/cse/configure-a-notification-playbook.png')} alt="Playbook for notification" style={{border: '1px solid gray'}} width="700"/>
1. Create an automation in Cloud SIEM to run the playbook:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu select **Cloud SIEM**. In the top menu of Cloud SIEM select **Configuration**, and then under **Integrations** select **Automation**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the top menu select **Configuration**, and then under **Cloud SIEM Integrations** select **Automation**. 
   1. At the top of the **Automation** tab, click **+ Add Automation**.
   1. For **Playbook**, select the playbook you created in the previous steps.
   1. For **Object (expects attributes for)**, select **Insight**.
   1. For **Execution**, select **Manually Done**.
   1. Click **Save**.
1. Run the automation:
   1. Select **Insights** from the main Cloud SIEM screen.
   1. Select an insight.
   1. Click the **Actions** button.
   1. Under **Insight Automation**, select the automation you created in the previous step (it will have the same name as the playbook). The playbook runs.
   1. To see the results of the run, click the **Automations** tab at the top of the insight.
   1. View the **Status** field to find out if the playbook has a status of **Success** or another status such as **Completed with errors**.
   1. Click **View Playbook** to see details of the playbook run. Each node in the playbook will show either **Success** or **Failed**.
   1. Click a node to download results of that node’s run.


## Advanced example: Configure a custom integration

The following example shows how to create a custom integration with an action that runs a script you provide. The custom integration and action are defined by YAML files. To learn how to build your own YAML files, see [Integration framework file formats](/docs/platform-services/automation-service/integration-framework/about-integration-framework/#integration-framework-file-formats).

The action uses [IP Quality Score](https://www.ipqualityscore.com/) to gather IP reputation information for enrichment. (This example shows how to add enrichment to an insight. To use the same action to add enrichment to entities, see [Add entity enrichment](#add-entity-enrichment) below.)

1. [Install the Automation Service Bridge](/docs/platform-services/automation-service/automation-service-bridge/). Because this example uses a custom integration, you must first install the Bridge before you proceed.
1. Obtain an API key from IP Quality Score:
    1. Create a free account on [IP Quality Score](https://www.ipqualityscore.com/create-account).
    1. Log in.
    1. Go to your [account settings](https://www.ipqualityscore.com/user/settings) and  copy the **API Key**. You will use this key later.
1. Create a new IP Quality Score integration:
    1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu, select **Automation** and then select **Integrations** in the left nav bar. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Integrations**. You can also click the **Go To...** menu at the top of the screen and select **Integrations**.
    1. Click the **+** icon at the top of the screen to the left of **Integrations**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-integration-button.png')} alt="Add integration button" style={{border: '1px solid gray'}} width="300"/>
    1. Download this file: <a href="/files/IP-Quality-Score-Test.yaml" target="_blank">IP-Quality-Score-Test.yaml</a>.
    1. In the **New Integration** dialog, click **Upload File**.
    1. Drag the file into the **Select File** box.
    1. Click **Upload**. An IP Quality Score integration is created.
    1. Open the new **IP Quality Score** integration.
    1. Hover your mouse over the **IP Quality Score** name and click the **Upload** button that appears.<br/><img src={useBaseUrl('img/cse/automation-examples-upload-button.png')} alt="Upload button" style={{border: '1px solid gray'}} width="250"/>
    1. In the **Upload** dialog, select **Action** in the **Type** field and click **Next**.
    1. Download this file: <a href="/files/IP-Reputation.yaml" target="_blank">IP-Reputation.yaml</a>.
    1. In the **Upload** dialog, click **Upload File**. 
    1. Drag the file into the **Select File** box.
    1. Click **Upload**. The **IP Reputation** action appears in the IP Quality Score integration.
1. Add the IP Quality Score integration resource:
    1. Click the **+** button to the left of **Resources**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-resource-button.png')} alt="Add resource button" style={{border: '1px solid gray'}} width="150"/>  
    1. Fill out the **Add Resource** dialog:
       * **Label**: Enter **IP Quality Score Resource**.
       * **API URL**: Enter `https://www.ipqualityscore.com/`.
       * **API Key**: Enter the API key you previously obtained from IP Quality Score.
       * **Connection Timeout (s)**: Leave the default value at **120**.
       * **Automation engine**: Select the Automation Bridge you installed locally as described in the first step of this example.
       * **Proxy options**: Select **Use no proxy**.
    1. Click **Save**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-resource-ip-quality-score.png')} alt="Add resource for IP Quality Score" style={{border: '1px solid gray'}} style={{border: '1px solid gray'}} width="400"/>   
1. Create the playbook:
    1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic).  In the main Sumo Logic menu, select **Automation > Playbooks**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Playbooks**. You can also click the **Go To...** menu at the top of the screen and select **Playbooks**.
    1. Click the **+** button to the left of **Playbook**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-playbook-button.png')} alt="Add playbook button" style={{border: '1px solid gray'}} width="300"/>
    1. Give your playbook a **Name**, such as **Custom Enrichment with IP Quality Score**.
    1. For **Type**, select **CSE**.
    1. Enter a **Description**.
    1. Click **Create**.
1. Select the input parameters for the playbook:
    1. Click the **Edit** button (pencil icon) at the bottom of the playbook view.
    1. On the **Start** node, click the **Edit** button (pencil icon).
    1. In the **Edit node** dialog, select **Insight** in the **Add one or more params as a playbook input** field. (If you want to create a playbook to add entity enrichment instead, see [Add entity enrichment](#add-entity-enrichment) below.)  
    1. Click **Update**.
1. Add a condition to validate IP addresses:
    1. Click the **Add Node** button (**+** icon) on the **START** node.
    1. In the **Add node** dialog, click **Condition**.
    1. Just below **Condition #1**, click the top **Select a value** in the dialog.
    1. Click [**Playbook inputs**](#playbook-inputs).
    1. Select **input.entity.entityType**.
    1. Click the bottom **Select a value** in the dialog.
    1. In **Get value**, type **_ip** and press **Enter**.
    1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-condition.png')} alt="Add a condition" style={{border: '1px solid gray'}} width="500"/>  
    1. Click and hold on the **FAILURE** (red) semicircle of the new condition node, and drag to the semicircle of the **END** node and release. This tells the playbook that if there are no valid IP addresses on entities, the playbook should end.
1. Add the “IP Reputation” action to the playbook:
    1. Click the **Add Node** button (**+** icon) on the **CONDITION** node.
    1. In the **Add node** dialog, click **Action**.
    1. In the **Integration** field, select **IP Quality Score**.
    1. In the **Action** field, select **IP Reputation**.
    1. To the right of the **IP** field, click the gear icon.
    1. Click [**Playbook inputs**](#playbook-inputs).
    1. Select **input.entity.value**.
    1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-ip-reputation-node.png')} alt="Add the IP Reputation node" style={{border: '1px solid gray'}} width="600"/>
1. Add the “Add Insight Enrichment” action to the playbook:
    1. Hover your mouse over the new **IP Reputation** node.
    1. Click the **Add Node** button (**+** icon) at the bottom of the **IP Reputation** node.
    1. In the **Add node** dialog, click **Action**.
    1. In the **Integration** field, select **Sumo Logic Cloud SIEM Internal**.
    1. In the **Type** field, select **Notification**.
    1. In the **Action** field, select **Add Insight Enrichment**.
    1. To the right of the **Insight ID** field, click the gear icon.
    1. Click [**Playbook inputs**](#playbook-inputs).
    1. Select **input.id**.
    1. In the **Enrichment name** field, enter the name of your playbook, for example, **Custom Enrichment with IP Quality Score**.
    1. To the right of the **Raw JSON** field, click the gear icon.
    1. Click **IP Reputation**.
    1. Select **output.raw**.
    1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-advanced-add-insight-enrichment-node.png')} alt="Add Insight Enrichment node" style={{border: '1px solid gray'}} width="600"/>  
    1. Click and hold on the right semicircle of the new **Add Insight Enrichment** node and drag to the semicircle of the **END** node and release. The playbook is complete.
1. Save the playbook:
    1. Click the **Save** button (floppy disk icon) at the bottom of the playbook view.
    1. To [test the playbook](/docs/platform-services/automation-service/automation-service-playbooks/#test-a-playbook), click the kebab button in the upper-right of the UI and select **Run Test**. 
    1. Click the **Publish** button (clipboard icon) at the bottom of the playbook view. The playbook should look like this:<br/><img src={useBaseUrl('img/cse/custom-integration-insight-enrichment.png')} alt="Custom playbook for insight enrichment" style={{border: '1px solid gray'}} width="700"/>
1. Create an automation in Cloud SIEM to run the playbook:
    1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu select **Cloud SIEM**. In the top menu select **Configuration**, and then under **Integrations** select **Automation**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the top menu select **Configuration**, and then under **Cloud SIEM Integrations** select **Automation**. 
    1. At the top of the **Automation** tab, click **+ Add Automation**.
    1. For **Playbook**, select the playbook you created in the previous steps.
    1. For **Object (expects attributes for)**, select **Insight**.
    1. For **Execution**, select **Manually Done**.
    1. Click **Save**.
1. Run the automation:
    1. Select **Insights** from the main Cloud SIEM screen.
    1. Select an **Insight**.
    1. Click the **Actions** button.
    1. Under **Insight Automation**, select the automation you created in the previous step (it will have the same name as the playbook). The playbook runs.
    1. To see the results of the run, click the **Automations** tab at the top of the insight.
    1. View the **Status** field to find out if the playbook has a status of **Success** or another status such as **Completed with errors**.
    1. Click **View Playbook** to see details of the playbook run. Each node in the playbook will show either **Success** or **Failed**.
    1. Click a node to download results of that node’s run.
    1. Go back to the insight and click the **Enrichments** tab to view the enrichments added by the automation.

### Add entity enrichment

The preceding example shows how to use a custom integration to add enrichment to an insight. To add enrichment to entities instead, use the same steps but with the following changes:

1. When you select the input parameters for the playbook, in the **Edit node** dialog, select **Entity** instead of **Insight** in the **Add one or more params as a playbook input** field.
1. When you add a condition to validate IP addresses, for [**Playbook inputs**](#playbook-inputs) select **input.entityType** instead of **input.entity.entityType**.
1. When you add the “IP Reputation” action to the playbook, for [**Playbook inputs**](#playbook-inputs) select **input.value** instead of **input.entity.value**.
1. Instead of adding the “Add Insight Enrichment” action to the playbook, add the “Add Entity Enrichment” action.

The resulting playbook should look like this:<br/><img src={useBaseUrl('img/cse/custom-integration-entity-enrichment.png')} alt="Custom playbook for entity enrichment" style={{border: '1px solid gray'}} width="700"/>

## Advanced example: Build a complex playbook

The following example pulls together elements of the [Simple example](#simple-example-configure-an-enrichment) and [Intermediate example](#intermediate-example-configure-a-notification) above. The resulting playbook runs an enrichment using VirusTotal, performs a Sumo Logic search, and sends an email notification.

1. Edit the VirusTotal OIF resource:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu, select **Automation** and then select **Integrations** in the left nav bar. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Integrations**. You can also click the **Go To...** menu at the top of the screen and select **Integrations**.  
   1. Select **VirusTotal OIF**.
   1. Hover your mouse over the resource name and click the **Edit** button that appears.<br/><img src={useBaseUrl('img/cse/automation-examples-virus-total-resource-edit-button.png')} alt="Resource edit button" style={{border: '1px solid gray'}} width="500"/>
   1. In the **Edit resource** dialog, enter the **API URL**: `https://www.virustotal.com`.
   1. Enter the **API Key**. See the [VirusTotal documentation](https://support.virustotal.com/hc/en-us/articles/115002100149-API) to learn how to obtain the API key. If you do not already have a VirusTotal account, you need to create one to get an API key.
   1. Click **Save**.<br/><img src={useBaseUrl('img/cse/automation-examples-virus-total-edit-resource.png')} alt="Edit resource" style={{border: '1px solid gray'}} width="400"/>
1. Edit the Sumo Logic resource:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the top menu select **Configuration**, and then under **Integrations** select **Automation**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the top menu select **Configuration**, and then under **Cloud SIEM Integrations** select **Automation**. 
   1. From the Automation screen, click **Manage Playbooks**. This opens the [Automation Service UI](/docs/platform-services/automation-service/about-automation-service/#automation-service-ui).
   1. Click **Integrations** in the navigation menu.
   1. Select **Sumo Logic**.
   1. Hover your mouse over the resource name and click the **Edit** button that appears.<br/><img src={useBaseUrl('img/cse/automation-examples-sumo-logic-cip-resource-edit-button.png')} alt="Resource edit button" style={{border: '1px solid gray'}} width="600"/>
   1. In the **Edit  resource** dialog, enter the **API URL** for your Sumo Logic core platform instance (for example, `https://api.us2.sumologic.com`). For the URL to use for your Sumo Logic instance, see [Sumo Logic Endpoints by Deployment and Firewall Security](/docs/api/about-apis/getting-started#sumo-logic-endpoints-by-deployment-and-firewall-security).
   1. [Create an access key](/docs/manage/security/access-keys#create-an-access-key) and copy the resulting access ID and access key.
   1. Enter the **Access ID** and the **Access Key**.
   1. Select your **Time Zone**.
   1. Click **Save**.<br/><img src={useBaseUrl('img/cse/automation-examples-edit-sumo-logic-resource.png')} alt="Edit a resource" style={{border: '1px solid gray'}} style={{border: '1px solid gray'}} width="400"/>
1. Create the playbook:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic).  In the main Sumo Logic menu, select **Automation > Playbooks**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the main Sumo Logic menu, select **Automation > Playbooks**. You can also click the **Go To...** menu at the top of the screen and select **Playbooks**.
   1. Click the **+** button to the left of **Playbook**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-playbook-button.png')} alt="Add playbook button" style={{border: '1px solid gray'}} width="300"/>
   1. In the **New playbook** dialog, give your playbook a **Name**.
   1. For **Type**, enter **CSE**.
   1. Enter a **Description**.
   1. Click **Create**.
1. Add the “IP Reputation V3” action to the playbook:
   1. Click the **Edit** button (pencil icon) at the bottom of the playbook view.
   1. Click the **Edit** button (pencil icon) on the **START** node.
   1. In the **Edit node** dialog, select **Insight** from the dropdown menu and click **UPDATE**.
   1. Click the **Add Node** button (**+** icon) on the **START** node.
   1. Select **Action**.
   1. In the **Add node** dialog, for **Integration** select **VirusTotal OIF**.
   1. Ensure that **Type** is **Enrichment**.
   1. For **Action**, select **IP reputation V3**.
   1. To the right of the **IPs** field, click the gear icon.
   1. Click [**Playbook inputs**](#playbook-inputs) and select **input.entity.value**.
   1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-ip-reputation-v3-add-node.png')} alt="Add node for IP Reputation V3" style={{border: '1px solid gray'}} width="500"/>
1. Add an enrichment action to the playbook:
   1. Hover your mouse over the **IP reputation V3** node and click the **Add Node** button (**+** icon).
   1. Select **Action**.
   1. In the **Add node** dialog, for **Integration**, select **Sumo Logic Cloud SIEM Internal**.
   1. For **Type**, select **Notification**.
   1. For **Action**, select **Add Insight Enrichment**.
   1. To the right of the **Insight ID** field click the gear icon.
   1. Click [**Playbook inputs**](#playbook-inputs) and select **input.readableId**.
   1. In the **Enrichment name** field type **VirusTotal IP reputation**.
   1. To the right of the **Raw JSON** field click the gear icon.
   1. Click **IP reputation V3** and select **output.raw**.
   1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-add-insight-enrichment-node.png')} alt="Add node for insight enrichment" style={{border: '1px solid gray'}} width="500"/>
   1. Click and hold on the right semicircle of the new **Add Insight Enrichment** node and drag to the semicircle of the **END** node and release.
1. Add a condition to validate IP addresses:
    1. Click the **Add Node** button (**+** icon) on the **Add Insight Enrichment** node.
    1. In the **Add node** dialog, click **Condition**.
    1. Just below **Condition #1**, click the top **Select a value** in the dialog.
    1. Under **Get value from a previous action**, select **IP Reputation V3**.
    1. Select **output.total_reputation**.
    1. Click the **>** (is greater than) operator.
    1. Click **Select a value**.
    1. In the **Get value** field, type **1** and press Enter.
    1. Click **Create**. <br/><img src={useBaseUrl('img/cse/automation-examples-complex-condition-1.png')} alt="Condition to validate IPs" style={{border: '1px solid gray'}} width="500"/>
1. Add the "Search Sumo Logic" action to the playbook:
    1. Click the **Edit** button (pencil icon) at the bottom of the playbook view.
    1. Click the **Edit** button (pencil icon) on the **START** node.
    1. In the **Edit node** dialog, select **Insight** from the dropdown menu and click **UPDATE**.
    1. Click the **Add Node** button (**+** icon) on **START**.
    1. In the **Add node** dialog, select **Action**.
    1. For **Integration**, select **Sumo Logic**.
    1. Ensure that **Type** is **Enrichment**.
    1. For **Action**, select **Search Sumo Logic**.
    1. In the **Query** box enter the search query you want to make in the Sumo Logic core platform. In the example below, a placeholder queries for a value obtained from the IP Reputation V3 node. For help with queries, see [General Search Examples Cheat Sheet](/docs/search/search-cheat-sheets/general-search-examples/).
    1. For **Last Period** select **15 Minutes** (or any time period you want).
    1. Click **Create**.<br/><img src={useBaseUrl('img/cse/automation-examples-search-sumo-logic-node-2.png')} alt="Add Search Sumo Logic node" style={{border: '1px solid gray'}} width="600"/>  
    1. Click and hold on the right semicircle of the new Search Sumo Logic node and drag to the semicircle of the **END** node and release.
1. Add the “Send Email” action to the playbook, which will run if no value is returned from the IP Reputation V3 node:
    1. Click the **Add Node** button (**+** icon) on the new **Condition**.
    1. In the **Add node** dialog, ensure **Failure** is selected under **Select an exit port**.
    1. Select **Action**.
    1. In the **Add node** dialog, for **Integration** select **Basic Tools**.
    1. Ensure that **Type** is **Notification**.
    1. For **Action** select **Send Email**.
    1. In **Recipients** enter your email address and press Enter.
    1. For **Subject** type a subject line for the email (for example, “Playbook completed”).
    1. In **Plain text** content enter the text you want to appear in the body of the email (for example, “Playbook completed. Click ‘View Playbook’  to see details”).
    1. Copy the plain text content into **HTML content** and add formatting if desired.
    1. Click **Create**. <br/><img src={useBaseUrl('img/cse/automation-example-playbook-4-send-email.png')} alt="Send Email action" style={{border: '1px solid gray'}} width="500"/>  
    1. Click and hold on the right semicircle of the new **Send Email** node and drag to the semicircle of the **END** node and release. The playbook is complete.
1. Save the playbook:
    1. Click the **Save** button (floppy disk icon) at the bottom of the playbook view.
    1. To [test the playbook](/docs/platform-services/automation-service/automation-service-playbooks/#test-a-playbook), click the kebab button in the upper-right of the UI and select **Run Test**. 
    1. Click the **Publish** button (clipboard icon) at the bottom of the playbook view. The playbook should look like this:<br/><img src={useBaseUrl('img/cse/automation-example-playbook-4.png')} alt="Complex playbook" style={{border: '1px solid gray'}} width="800"/> 
1. Create an automation in Cloud SIEM to run the playbook:
   1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the main Sumo Logic menu select **Cloud SIEM**. In the top menu select **Configuration**, and then under **Integrations** select **Automation**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the top menu select **Configuration**, and then under **Cloud SIEM Integrations** select **Automation**. 
   1. For **Playbook**, select the playbook you created in the previous steps.
   1. For **Object (expects attributes for)**, select **Insight**.
   1. For **Execution**, select **Manually Done**.
   1. Click **Save**.
1. Run the automation:
   1. Select **Insights** from the main Cloud SIEM screen.
   1. Select an insight.
   1. Click the Actions button.
   1. Under **Insight Automation**, select the automation you created in the previous step (it will have the same name as the playbook). The playbook runs.
   1. To see the results of the run, click the **Automations** tab at the top of the insight.
   1. View the **Status** field to find out if the playbook has a status of Success or another status such as **Completed with errors**.
   1. Click **View Playbook** to see details of the playbook run. Each node in the playbook will show either **Success** or **Failed**.
   1. Click a node to download results of that node’s run.
