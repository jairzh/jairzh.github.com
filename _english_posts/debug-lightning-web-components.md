---
title: "A Practical Guide to Debugging Lightning Web Components"
date: 2019-03-09 08:25:29 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2019/03/09/debug-lightning-web-components
translation_key: debug-lightning-web-components
description: "Use Debug Mode, breakpoints, Ignore List, and custom formatters to investigate LWC behavior."
categories: ["lwc"]
tags: ["lwc", "debugging"]
---

Lightning Web Components run as JavaScript in the browser, so Chrome DevTools is an essential part of debugging them. Salesforce adds a few complications: code can be minified, framework files can interrupt stepping, and values can appear through proxies.

The workflow from my original article still has three useful parts: enable readable code, set breakpoints in your component, and configure DevTools so that it shows the information you need.

## Enable Debug Mode for the right user

In Salesforce Setup, search for **Debug Mode** and open **Debug Mode Users**. Select the user doing the investigation and click Enable. Reload the page containing the component.

Debug Mode makes the component code easier to inspect. It is a troubleshooting setting, so enable it for the users who need it rather than assuming every user's session should use the same configuration. Salesforce explains the behavior in [Debug Mode](https://developer.salesforce.com/docs/platform/lwc/guide/debug-debug-mode.html).

## Find your component and set a breakpoint

Open Chrome DevTools with the browser menu or a keyboard shortcut, then select **Sources**. Use the source tree or Open File to locate the JavaScript file for the custom component.

Click a line number to set a breakpoint. Reload the page or repeat the interaction that triggers the code. When execution pauses, inspect local variables, the call stack, and the values entering your handler.

A breakpoint placed after the initial render will not tell you what happened before that point. For initialization problems, reload with the breakpoint already set. For an interaction problem, repeat the actual action rather than only refreshing the page.

## Ignore framework code deliberately

My 2019 article used Chrome's older term, **Blackboxing**. The current UI calls the feature **Ignore List**.

In DevTools Settings, open Ignore List and add patterns for framework or library files that are distracting you. You can also use a source file's context menu to ignore that particular script.

Be careful with broad patterns. A rule that excludes a large component directory can hide code you are trying to inspect. Confirm that your own component remains visible and available for stepping, and remove the rule when investigating a framework boundary.

The goal is to make stepping useful: you want to follow the behavior you own, while still being able to inspect the surrounding code when it matters.

## Enable custom formatters

In DevTools Settings, open Preferences and enable **custom formatters**. This can make proxied values easier to read when debugging in Salesforce.

Inspect the value before and after the line that changes it. A console entry by itself can be ambiguous when an object is inspected later; pausing at the relevant line gives you a clearer picture of the code's state.

Salesforce's [JavaScript troubleshooting unit](https://trailhead.salesforce.com/content/learn/modules/lwc-troubleshooting/get-ready-to-troubleshoot) covers source-file discovery, Ignore List, and custom formatters.

## Account for Lightning Web Security

The browser environment matters. When Lightning Web Security is enabled, some debugging behavior differs from older screenshots or examples. A console message's apparent source is not always the component file you expected.

Use Salesforce's [Debug with LWS Enabled](https://developer.salesforce.com/docs/platform/lightning-components-security/guide/lws-debug.html) guidance alongside the ordinary LWC debugging workflow. Do not infer the origin of an error solely from a framework filename in the console.

## A repeatable investigation

For a practical debugging session, I would follow this order:

1. Reproduce the problem with the relevant user and record.
2. Enable Debug Mode for that user and reload.
3. Locate the custom component's source file.
4. Set a breakpoint in the handler or lifecycle method involved.
5. Inspect inputs, state, and the call stack as the problem happens.
6. Use Ignore List and custom formatters to make the session readable.
7. Recheck the fix under the ordinary runtime settings.

The useful habit is to connect the visible symptom to a specific value and code path. Debug Mode and DevTools help you do that much faster than adding more console messages at random.

---

> Originally published on 2019-03-09. English edition revised on October 3, 2026. Updated Blackboxing terminology to Ignore List and added Lightning Web Security considerations. [Read the Chinese original](/blog/2019/03/09/debug-lightning-web-components).
