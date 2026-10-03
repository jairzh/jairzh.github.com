---
title: "Using AI to Build a Salesforce Learning Plan from Your Trailhead Profile"
date: 2026-01-18 12:56:57 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2026/01/18/ai-assisted-salesforce-learning-plan-2026
translation_key: ai-assisted-salesforce-learning-plan-2026
description: "A personal experiment using Trailhead evidence and AI to cross-check certification goals."
categories: ["ai"]
tags: ["certification", "learning-plan"]
image: /assets/images/wechat/ai-assisted-salesforce-learning-plan-2026/cover.jpg
---

Making a learning plan at the start of the year is a familiar routine. As a Salesforce professional, I normally combine Trailhead's learning paths with my own experience to choose the certifications worth pursuing.

For my 2026 plan, I tried a different approach: using ChatGPT and Gemini as advisers to cross-check my choices.

## 1. Start with the right data

My first attempt was to ask Gemini for recommendations directly. It suggested certifications I had already earned. The missing ingredient was my actual learning history.

My Trailhead profile already contained useful evidence: badges, superbadges, certifications, and the distribution of skills I had studied. However, giving Gemini the profile URL did not produce usable content in that attempt.

In the workflow described in my original article, I used ChatGPT with the Atlas browser to extract the profile into Markdown. The instruction was:

```text
Extract the information on this page in detail as Markdown.
Keep all the details. Do not summarize.
Write the output in English.
```

Manually copying the visible profile text works as an alternative. The important part is to check the resulting document for omissions; the model should be analyzing your history, not reconstructing it from guesses.

## 2. Give the analysis a reusable reference

I saved the extracted Markdown in a Google Doc, including the badge and superbadge information and the skill distribution. In the setup I used, Gemini could access the document as a reference.

That gave me a reusable snapshot rather than a long, one-off message. I could return to the same evidence when reviewing the plan later.

My analysis prompt was:

```text
Using this Trailhead profile's learning history and skill distribution,
and considering current developments in the Salesforce ecosystem,
recommend the two certifications most worth pursuing in 2026.
Explain the reasons for each recommendation.
```

For another person's plan, I would also include their role, years of experience, the work they expect to do, and their available study time. These details help distinguish a relevant next step from a generic list of popular certifications.

## 3. Compare the reasoning with your own judgment

The result surprised me. Gemini recommended exactly the same two certifications I had already selected manually. More interestingly, its reasoning matched my own: it identified gaps in my existing skills and explained why those areas were appropriate next steps.

The useful result was the method: give the model a verified learning history, then compare its recommendations and reasoning against your own goals.

Agreement is not proof that a plan is optimal. Before enrolling in an exam, check the current credential information, prerequisites, and exam guide on [Trailhead Credentials](https://trailhead.salesforce.com/credentials).

## A prompt for the wider plan

If you want to work on goals beyond certification, this is a useful starting point:

```text
Write my goals for the coming year.
Ask me the most important questions to craft the best possible set of goals.
```

The key lesson from this experiment was simple: **better evidence produces more relevant advice**. If the result feels generic, improve the input with your work history, responsibilities, and intended direction before asking for another recommendation.

---

> Originally published on 2026-01-18. English edition revised on October 3, 2026. Translated the prompts and clarified that the browser and document workflow describes the original experiment. [Read the Chinese original](/blog/2026/01/18/ai-assisted-salesforce-learning-plan-2026).
