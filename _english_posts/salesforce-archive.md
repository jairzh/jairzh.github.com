---
title: "Salesforce Archive: Retention, Access, and Cost Considerations"
date: 2026-01-31 18:12:53 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2026/01/31/salesforce-archive
translation_key: salesforce-archive
description: "Evaluate archiving policies, historical access, product scope, and the full cost of an archive solution."
categories: ["data"]
tags: ["archive", "storage", "architecture"]
image: /assets/images/wechat/salesforce-archive/cover.jpg
---

As a Salesforce org grows, old records compete with current work for storage and attention. Closed cases, completed activities, and historical custom-object records may need to remain available even when users rarely open them.

Deleting everything is often unsuitable, while keeping everything in the primary org can become expensive and cumbersome. That is the problem I wanted to explain in my original article about Salesforce Archive.

## Separate active data from historical data

The architectural idea is to distinguish two kinds of records:

- **Active data** supports current business processes and frequent access.
- **Historical data** is retained for reference, analysis, or the organization's retention requirements.

An archive policy determines when a record moves from the first group to the second. A backup serves a different purpose: recovering data after an incident. An archive needs an intentional access and retention model for historical information.

## Clarify which Archive product you are discussing

There is an important terminology issue when updating an older article. Salesforce's current [Archive product page](https://www.salesforce.com/platform/data-archiving-solutions/) describes automated policies, storage analysis, access to archived records, and restoration.

Salesforce also documents [Legacy Salesforce Archive](https://help.salesforce.com/s/articleView?id=archive_overview.htm&language=en_US&type=5), an external-storage solution with a different setup and access model. Do not assume that a reference to AWS, Azure, or Salesforce Connect applies equally to every offering called Archive.

For an evaluation, identify the exact product, entitlement, and storage architecture before designing the solution or estimating costs.

## Four capabilities to evaluate

### 1. Automated policies

Start with a business rule rather than a storage target. For example: a record is closed, older than the agreed retention threshold for active data, and no longer required by an active process.

Translate that rule into the product's supported policy configuration. The expression “older than three years” is a business condition, not a ready-to-run SOQL expression. Also consider related records; removing a parent without preserving needed context can leave historical data difficult to interpret.

### 2. Access from the user's workflow

Moving data out is only half of the problem. Users need a way to find and understand it afterward. Decide who may view archived data, where it appears, and when a record needs to be restored.

“Accessible” should be tested against real tasks. Can a support representative find an old case? Can they see the related information needed to interpret it? Does a report that once used active records still satisfy its requirement?

### 3. Storage and operating cost

The expected saving depends on the actual contract, data volume, growth, and access pattern. Include the archive subscription and the work needed to operate it, not just a comparison between storage unit prices.

An external-storage design may also involve infrastructure and connector costs. Those belong in the estimate only when they are part of the selected architecture.

### 4. Retention and governance

Define how long information remains available, who controls the policy, and how exceptions are handled. Retention, restoration, deletion, and records subject to a hold need a consistent operating process.

A configured retention period should implement your organization's requirements; it is not a substitute for deciding what those requirements are.

## When archiving is worth evaluating

Three useful signals from the original article remain relevant:

- Storage is dominated by historical records that users rarely access.
- Important queries or reports are becoming difficult to operate at the current volume.
- The business needs historical access, but keeping every record active is costly.

Investigate performance separately. A large record count does not by itself diagnose data skew or prove that archiving will fix a slow query. Access patterns, relationships, query design, and indexing still matter.

## Pricing: use an actual quote

The original article included a community price estimate whose units did not match its annual calculation. That estimate is intentionally omitted here.

Salesforce currently describes pricing as tailored to the customer's archiving needs. Obtain a quote for the specific product and volume, then compare the full operating cost with your alternatives. A historical community number is not a reliable budget for a new project.

## My recommendation

Begin with a retention and access policy, then evaluate the implementation. Some orgs need an archive product; others may have a simpler requirement for export, retention, and controlled deletion.

The goal is to keep the production org useful while preserving the historical information the business actually needs. That requires a clear access model as much as a place to store the data.

---

> Originally published on 2026-01-31. English edition revised on October 3, 2026. Distinguished current and legacy Archive offerings and removed the inconsistent community price estimate. [Read the Chinese original](/blog/2026/01/31/salesforce-archive).
