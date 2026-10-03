---
title: "Connecting WeChat QR Codes to Salesforce Campaign Attribution"
date: 2016-03-13 08:25:29 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2016/03/13/use-wechat-qrcode-in-your-salesforce-campaign
translation_key: use-wechat-qrcode-in-your-salesforce-campaign
description: "Turn event interactions into useful Salesforce Campaign data with attribution, identity matching, and registration."
categories: ["wechat"]
tags: ["wechat", "campaign", "integration"]
---

For a company running events in China, WeChat can be the entry point between an offline visitor and a Salesforce campaign. A QR code on a booth, invitation, or brochure gives an attendee a convenient action to take on their own phone.

My original article described using a parameterized WeChat Official Account QR code to connect that interaction to a Salesforce Campaign. The architecture is still worth explaining, particularly for readers who work with Salesforce but are less familiar with WeChat.

## Start with the CRM model

A Salesforce Campaign represents the marketing activity. Leads and Contacts participate through Campaign Member records, and member statuses describe the stages your team cares about—for example, registered or attended.

The manual workflow is familiar: collect details at the event, put them in a spreadsheet, and import them later. It creates a delay between meeting someone and making their participation available to the sales team.

A QR-based flow can shorten that gap. The key is to retain the event context all the way from the scan to the Campaign Member record.

## 1. Give each campaign entry point a reference

Create the Campaign in Salesforce, then associate a QR-code reference with it in the integration. The original design used the parameter carried by a WeChat Official Account QR code as that reference.

For a more detailed campaign, give different entry points their own references: a booth, a printed invitation, and a partner's brochure need not all share one code. Your mapping can associate each reference with the Campaign and the source location.

Keep the payload simple. It should identify the entry point, not expose a person's details. Confirm that the account type and enabled interfaces support the QR and callback capabilities your implementation requires.

## 2. Receive and validate the interaction

The integration receives the relevant event from WeChat, validates it, and resolves the campaign reference. The exact payload and behavior depend on the Official Account interfaces and whether the interaction involves a new or existing follower.

At the design level, the useful information is:

- Which entry point was used?
- Which WeChat identity is associated with the interaction, if the interface supplies one?
- When did the interaction happen?
- Has this interaction already been processed?

Do not equate a scan with a fully identified Lead. The identity available to the integration is not necessarily an email address, a phone number, or an existing Salesforce Contact.

## 3. Ask for the details you actually need

The original article added a registration page to collect information unavailable from the scan itself. For example, an attendee could open a check-in link and provide their name, company, and business email.

Carry the campaign context into that page using a server-validated reference. When the attendee submits the form, explain how the information will be used and apply the consent requirements for that workflow.

Then resolve the person's identity:

1. Check for an existing Lead or Contact using your organization's matching rules.
2. Create a record only when the rules indicate that it is appropriate.
3. Add or update the corresponding Campaign Member.
4. Preserve the entry-point attribution you need for reporting.

This is where the integration earns its value. It turns an anonymous or partially identified interaction into useful CRM data without creating a new duplicate for every scan.

## 4. Define what each status means

Scanning a code, submitting a registration form, and attending an event are different actions. Decide which action sets each Campaign Member status.

For example, a form submission can establish registration. Attendance may require a distinct check-in step. If every scan is marked “Attended,” the resulting report will overstate the event's outcome.

Repeated scans and callback retries should be safe. A durable interaction reference and idempotent upsert logic are useful design tools; the implementation should not create duplicate members simply because the same message arrives again.

## Where Charket fits

The original article pointed readers to Charket on AppExchange to explore a Salesforce–WeChat integration. The campaign pattern can also be implemented through another integration architecture; its value comes from preserving attribution, matching identity, and recording the correct CRM outcome.

The original 2016 implementation is a historical example, not a statement that every current Official Account has identical API access. Verify account capabilities, callbacks, and messaging rules before applying the design. Tencent's [parameterized QR-code documentation](https://developers.weixin.qq.com/doc/offiaccount/Account_Management/Generating_a_Parametric_QR_Code.html) is the relevant starting point.

For teams marketing in China, the useful opportunity is to connect an action people already understand—scanning a WeChat QR code—to a campaign workflow the sales team can act on.

---

> Originally published on 2016-03-13. English edition revised on October 3, 2026. Adapted the historical implementation into an architectural guide; exact current account permissions and API payloads must be checked for the implementation. [Read the Chinese original](/blog/2016/03/13/use-wechat-qrcode-in-your-salesforce-campaign).
