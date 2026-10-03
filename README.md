# Jair Zheng's Blog

My Blog: [jairzh.com](https://www.jairzh.com)

## Languages

Chinese posts remain in `_posts` with their existing `/blog/...` URLs and pagination.
The English home page is `/en/`; English articles live in `_english_posts` and
do not appear in the Chinese post list, search results, or archives.

To add an English translation:

1. Add the same `translation_key` to both versions of the article.
2. Create `_english_posts/<slug>.md` with `title`, `date`, `description`,
   `categories` and `tags` (as YAML arrays), and an explicit
   `permalink: /en/blog/YYYY/MM/DD/<slug>` matching the original publication date.
3. Reuse the original `image` when appropriate; the post layout renders it as
   the cover, so do not repeat it at the start of the body.

English articles inherit `layout: post`, `author: jair`, and `lang: en`.
Matching `translation_key` values connect the language switcher and reciprocal
`hreflang` links. Pages without translations link to the other language's home
page. Each language version keeps its own canonical URL. English categories,
tags, search results, and RSS (`/en/feed.xml`) use only the English collection.

Open a pull request for structural changes and translations. Merging into
`master` triggers the existing GitHub Pages deployment.

## Create a new post

1. `jekyll serve --watch`

2. Add a `.md` file in `_posts`.

3. YAML front matter
    - featured post `- featured:true`
    - exclude featured post from “All Posts” loop to avoid duplicated posts `- hidden:true`
    - post image `- image: assets/images/mypic.jpg`
    - external post image `- image: "https://externalwebsite.com/image4.jpg"`
    - page comments `- comments:true`
    - meta description (optional) `- description: "this is my meta description"`


### YAML Post Example:

```yaml
---
layout: post
title:  "We all wait for summer"
author: john
categories: [ Jekyll, tutorial ]
image: assets/images/5.jpg
description: "Something about this post here"
---
```

### YAML Page Example:

```yaml
---
layout: page
title: Mediumish Template for Jekyll
comments: true
---
```

### Rating:

```yaml
---
layout: post
title:  "We all wait for summer"
author: john
categories: [ Jekyll, tutorial ]
image: assets/images/5.jpg
description: "Something about this post here"
rating: 4.5
---
```

### Table of Contents:

```yaml
---
layout: post
title:  "Education must also train one for quick, resolute and effective thinking."
author: john
categories: [ Jekyll, tutorial ]
image: assets/images/3.jpg
beforetoc: "Markdown editor is a very powerful thing. In this article I'm going to show you what you can actually do with it, some tricks and tips while editing your post."
toc: true
---
```
