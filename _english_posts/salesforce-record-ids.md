---
title: "Salesforce 15- vs. 18-Character IDs: Avoiding Integration Bugs"
date: 2016-04-16 09:25:29 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2016/04/16/salesforce-record-ids
translation_key: salesforce-record-ids
description: "Understand case-sensitive and case-safe Salesforce IDs, their suffix calculation, and conversion options."
categories: ["apex"]
tags: ["salesforce", "record-ids", "integration"]
---

A Salesforce record ID identifies one record. It is used in queries, exports, integrations, and URLs, so a small misunderstanding about its format can turn into a difficult data-matching bug.

Salesforce IDs have two familiar forms: 15 characters and 18 characters. Both refer to the same underlying record, but they behave differently when a system compares text without respecting case.

## Why there are two formats

A 15-character ID is case-sensitive. The letters in these two example strings differ:

```text
00Qi000000g4YE2
00Qi000000g4ye2
```

A case-insensitive comparison treats them as the same text even though the original 15-character values are different. That is a problem when joining Salesforce exports in spreadsheets or another system with case-insensitive keys.

The 18-character form adds three characters encoding the capitalization of the first 15. That makes it suitable for matching in case-insensitive systems. Salesforce calls this form **case-safe**. [Salesforce's conversion guidance](https://help.salesforce.com/s/articleView?id=How-to-convert-a-15-character-id-to-a-18-character-id-1327109385626&language=en_US&type=1) describes both formats.

Case-safe does not mean “change the capitalization whenever you like.” Preserve IDs exactly as received, and do not lowercase them before generating the suffix. If the original capitalization has already been lost, the conversion cannot infer it from a 15-character string alone.

## What the prefix tells you

The first three characters are the object's key prefix. For example, a familiar prefix can help identify the object type of a record you are investigating.

Avoid building business logic around assumptions about the rest of the ID, or hard-coding custom-object prefixes across organizations. In Apex, use the ID's object type when you need it:

```apex
Id recordId = '001000000000001';
Schema.SObjectType objectType = recordId.getSObjectType();
```

This is a format example, not the ID of a record to query in your org.

## How the extra three characters are calculated

Split the 15-character ID into three groups of five. Within each group, mark each uppercase letter with a bit. The first character corresponds to bit zero, the second to bit one, and so on. Lowercase letters and digits contribute zero.

That produces a number from 0 to 31. Use it as an index into:

```text
ABCDEFGHIJKLMNOPQRSTUVWXYZ012345
```

Append the three resulting characters, one for each group, to the original ID. This equivalent bit-based explanation avoids having to reverse the groups manually.

## Conversion options

### Formula field

For an export or report, a Text formula field can expose the case-safe ID:

```text
CASESAFEID(Id)
```

Salesforce documents the function in [CASESAFEID](https://help.salesforce.com/s/articleView?id=customize_functions_casesafeid.htm&language=en_US&type=5).

### Apex

Use the platform's `Id` type instead of reimplementing the suffix calculation:

```apex
Id recordId = Id.valueOf(inputId);
String outputId = String.valueOf(recordId);
```

Handle invalid input at the integration boundary. A value having the right length does not establish that the record exists or that the current user may access it.

### JavaScript

If you need the calculation outside Salesforce, this implementation converts a correctly capitalized, 15-character alphanumeric input:

```javascript
function to18CharacterId(id) {
  if (typeof id !== "string" || !/^[a-zA-Z0-9]{15}$/.test(id)) {
    throw new TypeError("Expected a 15-character Salesforce ID");
  }

  const alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ012345";
  let suffix = "";

  for (let group = 0; group < 3; group++) {
    let mask = 0;
    for (let position = 0; position < 5; position++) {
      const character = id[group * 5 + position];
      if (character >= "A" && character <= "Z") {
        mask |= 1 << position;
      }
    }
    suffix += alphabet[mask];
  }

  return id + suffix;
}
```

The format check deliberately accepts only 15 characters. It does not validate an existing 18-character suffix or look up a Salesforce record.

## My recommendation

Use 18-character IDs at integration and export boundaries, preserve capitalization, and normalize the format before joining datasets. Check format, record existence, and authorization separately. Removing this ambiguity early is easier than diagnosing mismatched data later.

---

> Originally published on 2016-04-16. English edition revised on October 3, 2026. Updated the conversion example and removed legacy tool recommendations and assumptions about internal ID structure. [Read the Chinese original](/blog/2016/04/16/salesforce-15-and-18-char-reocrd-ids).
