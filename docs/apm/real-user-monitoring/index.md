---
slug: /apm/real-user-monitoring
title: Real User Monitoring (RUM)
description: Real User Monitoring (RUM) gives you insights into the quality of customer interactions with the digital interfaces of your business.
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import Iframe from 'react-iframe';
import DocCardList from '@theme/DocCardList';
import {useCurrentSidebarCategory} from '@docusaurus/theme-common';

<img src={useBaseUrl('img/icons/apm.png')} alt="icon" width="60"/>

Real User Monitoring (RUM) gives you the ability to understand how users interact with the digital interfaces of your business and if their experience is satisfactory or not. This open-source-powered and flexible capability brings you full visibility into what’s happening in your user's browser while interacting with your web applications.

RUM provides you visibility into end-to-end individual user transactions to quickly understand the user experience. You'll get insights into delays that occurred on the client, overall end-to-end transaction times, network timings, rendering events, and can perform high-level monitoring, alerting, as well as troubleshooting any potential slow-downs. You have full details about the specific performance of top user cohorts, their geographical locations, browsers, and operating systems. You can also fully understand the overall experience of all users and transactions of your digital business, all the time.

## How it works

The [Sumo Logic OpenTelemetry auto-instrumentation for JavaScript](https://github.com/SumoLogic/sumologic-opentelemetry-js) library enables RUM data collection in the form of OpenTelemetry-compatible traces and logs directly from the browser. It gathers information about the load, execution, and rendering of your JavaScript applications and records information about browser-to-backend performance of every user transaction in real time, without sampling.

This data is gathered directly from your end-user devices and displayed as individual spans representing user-initiated actions (like clicks or document loads) at the beginning of each trace, reflecting its request journey from the client throughout the whole application and back. This includes any unhandled errors, exceptions, and console errors generated by the browser.

All data collected is compatible with OpenTelemetry and doesn't use proprietary vendor code. Real user monitoring supports document load actions as well as XHR communication and route changes for single-page app navigation. The full list of functionalities and configuration is available in the [Sumo Logic OpenTelemetry auto-instrumentation for JavaScript](https://github.com/SumoLogic/sumologic-opentelemetry-js) README file.

:::sumo Micro Lesson
See Real User Monitoring in action.

<Iframe url="https://fast.wistia.net/embed/iframe/jfptjgwql1?web_component=true&seo=true&videoFoam=false"
  width="854px"
  height="480px"
  title="Micro Lesson: Real User Monitoring (RUM) 2.0 Video"
  id="wistiaVideo"
  className="video-container"
  display="initial"
  position="relative"
  allow="autoplay; fullscreen"
  allowfullscreen
/>

:::

## What You'll Need

| Account Type | Account Level |
|:--|:--|
| Credits | Enterprise Operations and Enterprise Suite. Essentials get up to 5 GB a day. |

Access Traces to confirm that your Sumo Logic service package has been upgraded to include Traces and Real User Monitoring.

[**Classic UI**](/docs/get-started/sumo-logic-ui-classic). To access Traces, go to the **Home** screen and select **Traces**.

[**New UI**](/docs/get-started/sumo-logic-ui/). To access Traces, in the main Sumo Logic menu select **Observability**, and then under **Application Monitoring** select **Transaction Traces**. You can also click the **Go To...** menu at the top of the screen and select **Transaction Traces**.
 

## Guides

<DocCardList items={useCurrentSidebarCategory().items}/>
