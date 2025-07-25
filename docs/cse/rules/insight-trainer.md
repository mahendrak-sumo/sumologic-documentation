---
id: insight-trainer
title: Improve Rules with Insight Trainer
sidebar_label: Improve Rules with Insight Trainer
description: The Cloud SIEM Insight Trainer dashboard recommends adjustments to rules to improve insight generation.  
keywords:
  - cloud siem
  - insight trainer
  - rule tuning
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import Iframe from 'react-iframe';

[Cloud SIEM - Insight Trainer](/docs/integrations/sumo-apps/cse/#cloud-siem---insight-trainer) is a dashboard in the Enterprise Audit - Cloud SIEM app. Insight Trainer offers suggestions for making adjustments to rules, such as writing rule tuning expressions and changing severities. Implementing the recommendations causes rules to be more effective at creating high-fidelity signals, resulting in generation of more meaningful insights. 

:::sumo Micro Lesson

Watch this micro lesson to learn how to use the Insight Trainer dashboard.

<Iframe url="https://fast.wistia.net/embed/iframe/9t416emj4w?web_component=true&seo=true&videoFoam=false"
  width="854px"
  height="480px"
  title="Micro Lesson: Cloud SIEM Insight Trainer Video"
  id="wistiaVideo"
  className="video-container"
  display="initial"
  position="relative"
  allow="autoplay; fullscreen"
  allowfullscreen
/>

:::

## About Insight Trainer

When you [close an insight](/docs/cse/administration/manage-custom-insight-resolutions/#close-an-insight-using-a-custom-resolution), you can give it one of the following built-in [insight resolutions](/docs/cse/administration/manage-custom-insight-resolutions/): 

* **Duplicate**. The insight has triggered before on the same entity and is a duplicate.
* **False Positive**. A false alarm, possibly due to an error in the detection logic of the rule or inapplicability to your environment.
* **No Action**. A valid detection, but no action is necessary due to effective containment measures. Rules participating in repeated No Action insights may also be good tuning expression candidates.
* **Resolved**. A valid detection where investigation was necessary.

As you resolve insights, you may find you have a high ratio of False Positive and No Action resolutions compared to Resolved resolutions. By reducing the number of signals that produce insights that turn out to be false positives or require no action, you can produce more reliable insights. 

:::note
For Insight Trainer to work correctly, it needs a balanced set of insights in a closed state with an appropriate mix of resolution statuses, such as **False Positive**, **No Action**, and **Resolved**. Unbalanced datasets can occur if resolutions are unrealistically assigned, for example, if all insights are closed with a single closed state such as **False Positive**.
:::

You could use trial-and-error to tune rules, but the Insight Trainer dashboard provides a data-driven approach. Once a week, the Insight Trainer provides fresh recommendations based on analysis of the last 60 days of data. Machine learning and AI learn historical patterns from your own data to suggest rule severity adjustments that minimize false positives without missing out on actual incidents. To see fresh recommendation every week, you must make suggested tuning adjustments at least once every two weeks. If you implement the suggested changes on a regular basis, the number of false positive resolutions can be greatly reduced. 

The dashboard makes two kinds of suggestions, either a “tunability” score to help you write tuning expressions for entities, or recommendations to change the severity level of rules. We recommend you first look at rules with the highest tunability score and assess the dominant entities (like users and IPs) to write tuning expressions for. Only after implementing tuning expressions do we recommend you adjust rule severities. This [suggested workflow](#suggested-workflow) will give you the best results over time.

## Cloud SIEM - Insight Trainer page

After installing the [Enterprise Audit - Cloud SIEM app](/docs/integrations/sumo-apps/cse), access the [Cloud SIEM - Insight Trainer](/docs/integrations/sumo-apps/cse/#cloud-siem---insight-trainer) dashboard by clicking the [Library](/docs/get-started/library) icon in the left nav bar.

The dashboard has the following sections:
* [Filters](#filters)
* [Recommendations Summary](#recommendations-summary)
* [Recommended Rule Severities](#recommended-rule-severities)

### Filters

Use the fields at the top of the page to filter the kinds of recommendations you want to view.

<img src={useBaseUrl('img/cse/cloud-siem-insight-trainer-filters.png')} alt="Insight Trainer filters" width="800"/>

1. **minimize**. The types of [insight resolutions](/docs/cse/administration/manage-custom-insight-resolutions) you want to minimize:
   * **False positive**. Display recommendations only for reducing False Positive resolutions.
   * **False positive & No action**. Display recommendations for reducing both False Positive and No Action resolutions. 
1. **show_rules**. The types of rules you want to show recommendations for:
   * **All rules**. Show all rules that were analyzed, not just rules with recommendations.
   * **Rules with severity recommendations**. Provide recommendations only for those rules with suggested changes to their severity.
1. **deployment**. The deployment whose rules you want recommendations for. 
1. **domain**. The domain whose rules you want recommendations for.
1. **Date Range**. Once a week, the Insight Trainer provides fresh recommendations. The date range displayed is the model training period and is read-only. Model retraining is weekly, based on a rolling history of your insights data. The funnel shows the number of insights eligible for recommendations. For example, in the image above, 24 insights out of 30 are eligible for recommendations. In many cases, insights are eligible for recommendations because they originate from rules that have a static severity that can be updated.
The funnel depicts algorithmic insights that remain after filtering insights based on:
   * Date range
   * Insights containing dynamic severity rules
   * Non-algorithmic detection (that is, rule and user insights)
   * Duplicate insights
   * Data completeness 
1. **Insight Source**. The primary source of insights (for example, by algorithm, rule, or user).

### Recommendations Summary

This panel summarizes the changes to insight resolutions if you implement the recommended rule changes. 

<img src={useBaseUrl('img/cse/cloud-siem-insight-trainer-recommendations-summary.png')} alt="Insight Trainer Recommendations Summary" width="800"/>

1. **Total Eligible Insights (prior to optimization)**. The number of insights that could be reduced.
1. **Optimized Labeled Insights (OLI)**. The number of insights remaining with a resolution label.
1. **Forecasted Unlabeled Insights (FUI)**. The number of insights that will no longer have a resolution label.
1. **Total Optimized Insights (FUI + OLI)**. The total number of insights with a resolution label either added or removed.
1. **False Positive Rate: Eligible Insights**. The false positive rate for insights that are eligible for optimization.
1. **False Positive Rate: Optimized Insights**. The false positive rate for insights after optimization.
1. **Insight Counts by Resolution: Eligible v. Optimized Labeled Insights**. Bar chart showing the relative number of Resolved versus False Positive resolutions that would result if you implement the recommended rule changes. 


### Recommended Rule Severities

This panel shows the rules recommended for severity changes. 

<img src={useBaseUrl('img/cse/cloud-siem-trainer-recommended-rule-severities.png')} alt="Insight Trainer Recommend Rule Severities" width="800"/>

1. **rule**. The rule recommended to change the severity level for.
1. **current_severity**. The rule’s current severity level.
1. **recommended_severity**. The recommended severity level.
1. **signal_count**. The number of signals generated by the rule in the time period set in the dashboard’s filter.
1. **tunability**. The score indicating how good a candidate the rule is for having a tuning expression change. The closer to 100 the score is, the better a candidate is. Click the score to get a list of the entities that you can add tuning expressions for.
1. **eligible_count**. The number of insights that use the rule that had False Positive and No Action resolutions for the time period set in the dashboard’s filter. 
1. **optimized_count**. The number of insights anticipated to have False Positive and No Action resolutions for the set time period if the rule’s severity is updated per the recommendation.

Before making changes to a rule’s severity, check the rule’s tunability score to see if it is a good candidate for a [rule tuning expression](/docs/cse/rules/rule-tuning-expressions) change, and click the score to get a list of entities to tune the rule for. Scores closer to 100 are better candidates. Because a small number of entities can cause many False Positive and No Action resolutions, it’s preferable to write a tuning expression for a rule rather than change its severity. This will result in a more long-term reduction in false positive insights. 

In general, fewer insights are generated if you decrease severities on rules, and more insights are generated if you increase severities on rules. If more insights are generated because you increase severity, the incrementally changed insights no longer have a resolution label (are “unlabeled”). 

## Suggested workflow

Following is the suggested workflow to use the Insight Trainer dashboard:
1. Review severity recommendations once a week. The Insight Trainer provides fresh recommendations weekly, provided that you regularly implement recommendations. 
1. Look at the rules with the highest tunability scores and assess the dominant entities to write [rule tuning expressions](/docs/cse/rules/rule-tuning-expressions) for (like users and IP addresses).
1. Write tuning expressions for entities, where possible.
1. Adjust rule severities if needed.

We suggest adjusting rule severities to the recommended levels only after you have written rule tuning expressions and seen how they result in lowering false positives. The algorithm adjusts its recommendations continuously. So if at first you do not see your false positives change much, wait a few days, and you will notice new recommendations.  
