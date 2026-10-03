---
title: "Four Practical Patterns for Reactive Salesforce Screen Flows"
date: 2026-05-24 11:33:23 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2026/05/24/salesforce-reactive-screen-flow
translation_key: salesforce-reactive-screen-flow
description: "Connect screen components with formulas, calculated dates, and conditional visibility."
categories: ["flow"]
tags: ["screen-flow", "reactivity"]
image: /assets/images/wechat/salesforce-reactive-screen-flow/cover.jpg
---

Screen Flow reactivity lets components on the same screen respond to one another as the user works. A checkbox can disable an input, a formula can turn a subject into a suggested priority, and several inputs can contribute to a calculated date—all without asking the user to click Next.

The useful pattern is to treat a screen as a small interactive application. Components supply values, formulas transform them, and other components use the results. Here are four examples from my original article.

## 1. Let one component control another

Add a Checkbox component called `Confirmation` and a Text component called `Subject`. Bind the Text component's Disabled property to the checkbox's Boolean value.

When the user selects the checkbox, the subject becomes read-only. When they clear it, they can edit the subject again. This is useful for an explicit confirmation step before submission.

The connection is direct: the checkbox is the source, and the Disabled property is the target. No formula is needed if the source already provides the right type.

## 2. Use a formula as a routing layer

A formula becomes useful when the target needs a different value from the one the user entered. Suppose the subject should suggest a high priority when it contains “urgent.” A Text formula resource can express that rule:

```text
IF(
    CONTAINS(LOWER({!Subject}), "urgent"),
    "High",
    "Normal"
)
```

Reference the formula as the default value of a compatible priority component. Its choice values must match `High` and `Normal`; labels and stored values are not necessarily identical.

This produces a suggested priority, not a complete business validation rule. Check how the component behaves after the user manually changes it, and decide whether that choice should take precedence over the suggestion.

## 3. Combine inputs to calculate a date

The original example combines a priority-based number of days with a Slider adjustment. Configure the priority choice to provide a numeric value and create a Date formula such as:

```text
TODAY()
+ BLANKVALUE({!PriorityDays}, 0)
+ BLANKVALUE({!AdjustmentDays}, 0)
```

Use the result as the default value for a compatible Date component. A user can change either input and see the proposed due date update on the same screen.

Explicit defaults matter. The formula should behave sensibly before all inputs have values. Also, this example adds calendar days; a requirement for business days needs a different calculation.

## 4. Show the right message at the right time

Use conditional visibility to show an approval warning when the Slider exceeds a threshold. For example:

- Show the warning when `AdjustmentDays > 3`.
- Show the normal guidance when `AdjustmentDays <= 3`.

These conditions cover the boundary value of exactly three days. Otherwise, it is easy to accidentally create a state where neither message appears.

The result is immediate guidance: the screen tells the user when their choice requires attention, rather than waiting until they submit it.

## What to check before shipping

Reactivity is supported by particular components, properties, and formula functions—not by every resource in Flow Builder. Check the flow's API version and test initial values, cleared values, user overrides, and returning to the screen. Salesforce lists the supported [components](https://help.salesforce.com/s/articleView?id=sf.flow_build_reactive_screen_flow_components.htm&language=en_US&type=5) and [formula operators](https://help.salesforce.com/s/articleView?id=sf.flow_build_reactive_screen_flow_formula_operators.htm&language=en_US).

My takeaway is that formulas can serve as the small logic layer between screen components. A handful of clear connections can make a flow feel responsive without adding a separate screen for every decision.

---

> Originally published on 2026-05-24. English edition revised on October 3, 2026. Expanded with illustrative formulas and implementation considerations. [Read the Chinese original](/blog/2026/05/24/salesforce-reactive-screen-flow).
