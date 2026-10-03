---
title: "Why Images Fail in Salesforce Emails\u2014and How to Fix Them"
date: 2016-05-16 09:25:29 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2016/05/16/adding-image-in-email-template
translation_key: adding-image-in-email-template
description: "Choose an accessible image source, distinguish template types, and test the delivered email."
categories: ["admin"]
tags: ["email", "templates"]
---

An email template can look correct inside Salesforce and still show a broken image to its recipient. The usual reason is that the image URL works for a signed-in user but is not a usable image source for an external email client.

My original article explained this through Salesforce Classic templates and Documents. The principle remains useful: **an externally hosted image needs a stable, accessible URL**, and the actual delivered message needs to be tested.

## Choose the method for the template you use

Lightning templates and enhanced letterheads have image capabilities that differ from older Classic HTML and Visualforce templates. Start with the image tools in the editor for the template type you are using.

Salesforce documents that saving a template can convert Salesforce content-document links into content-asset links. That is one reason manually copying a file preview URL is not equivalent to inserting an image through the supported editor. See [Using Images in Emails, Email Templates, and Enhanced Letterheads](https://help.salesforce.com/s/articleView?id=sales.email_images.htm&language=en_US&type=5).

For a custom HTML template, you can reference a suitable hosted image explicitly:

```html
<img
  src="https://cdn.example.com/email/logo.png"
  alt="Company logo"
  width="240"
  style="display:block; max-width:100%; height:auto;"
>
```

Replace the example URL with your actual image. The `src` should identify the image response, not a page that previews a file.

## Check access without your Salesforce session

Open the final image URL in a browser session that is not signed in to your org. If it opens a login page or a file preview, it is not ready for the external recipient.

This check is necessary, but it is not the entire email test. A recipient's client may block remote images until the user enables them, and different clients render HTML differently. Provide useful alternative text and keep the message understandable when images are hidden.

Only use public hosting for content intended to be publicly retrievable, such as a marketing logo. A picture's presence in an internal Salesforce record does not establish that it is suitable for public distribution.

## The historical Documents approach

In the Classic workflow from the original article, the steps were:

1. Upload the image to a Documents folder.
2. Enable **Externally Available Image** for the document.
3. Obtain its image URL.
4. Reference that URL in the HTML or Visualforce template.
5. Verify it without an authenticated org session.

The resulting URL used a form like this:

```text
https://<salesforce-host>/servlet/servlet.ImageServer?id=<document-id>&oid=<org-id>
```

This is background for an existing Classic implementation, not a universal URL recipe for Lightning Files. Do not manufacture a download URL by replacing parts of a file preview link. Follow the supported image method for the particular storage and template type.

Salesforce's [Classic template image instructions](https://help.salesforce.com/s/articleView?id=sf.email_template_images.htm&language=en_US&type=5) describe that workflow and external image URLs.

## Test a delivered message

After saving the template, send it to an external mailbox and inspect the result. A useful test covers:

- The image loads when remote images are allowed.
- The content remains readable when they are blocked.
- The image has an appropriate size on desktop and mobile.
- The URL does not depend on your login or an unexpectedly short expiration.
- The received HTML references the intended image.

If it fails, inspect the image URL and response first. Authentication redirects, an expired link, or a file-preview response are different problems from an email client hiding remote content.

The practical lesson is to validate the recipient's experience, not just the template editor. A stable source and a real delivery test will find problems that an internal preview cannot.

---

> Originally published on 2016-05-16. English edition revised on October 3, 2026. Updated guidance for Lightning templates and retained the Documents approach as historical Classic context. [Read the Chinese original](/blog/2016/05/16/adding-image-in-email-template).
