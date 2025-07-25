---
id: global-intelligence-security-insights
title: Global Intelligence for Security Insights
description: Insight Confidence scores, predicted by Sumo Logic’s Global Intelligence machine learning model, help you triage and prioritize insights.
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import Iframe from 'react-iframe';

This page describes Global Intelligence for security insights, implemented in Cloud SIEM as Global Confidence scores. This feature helps security analysts triage and prioritize insights.

:::sumo Micro Lesson

Watch this micro lesson to learn more about Global Intelligence for insights.

<Iframe url="https://fast.wistia.net/embed/iframe/d5ue1hgvdw?web_component=true&seo=true&videoFoam=false"
  width="854px"
  height="480px"
  title="Micro Lesson: Cloud SIEM Global Intelligence for Security Insights Video"
  id="wistiaVideo"
  className="video-container"
  display="initial"
  position="relative"
  allow="autoplay; fullscreen"
  allowfullscreen
/>

:::

## What is a Global Confidence score?
An insight’s Global Confidence score represents a level of confidence, predicted by Sumo Logic’s Global Intelligence machine learning model, that the insight is actionable. 

<img src={useBaseUrl('img/cse/closeup.png')} alt="Global confidence score example" style={{border: '1px solid gray'}} width="400"/>

The score is generated based on the underlying pattern of signals in an insight. The model compares this pattern to previously observed patterns from insights that were closed with either a **False Positive** or **Resolved** resolution. The model does such comparisons broadly—across the global installed base of Cloud SIEM customers—so it can generate a Confidence score based on the patterns seen at one customer when encountered at another. In addition to leveraging the patterns discovered across the Cloud SIEM installed base, the model customizes scores for insights in your account based on your customized content, including tuned and custom rules.

:::tip Fear not
All information used by the model is anonymized and no customer-confidential information is processed or retained.
:::


The score is on a scale of 0 to 100. A higher score indicates higher confidence that the insight is actionable. If the model does not have enough information, it will not make a prediction and no score will be listed (you’ll see either “No prediction” or “N/A”).

## Prerequisites for using Global Confidence scores
The only prerequisite for taking full advantage of Confidence scores is to make sure your content is available to Sumo Logic’s machine learning model. If you do not close insights with an appropriate resolution, the model won’t be able to consider your content and may not be able to generate Global Confidence scores for your insights. To take full advantage of this feature, make sure you close your insights as False Positive or Resolved.

## Using Global Confidence scores
The Global Confidence score is a valuable data point to consider when prioritizing which insights to triage first.

An insight’s Confidence score is shown for each insight on the insights list page. You can sort the insight list by the Global Confidence score, as well as by Severity.

<img src={useBaseUrl('img/cse/Confidence-Screenshot.png')} alt="Global confidence screen image example" width="800"/>
 
