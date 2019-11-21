# Jair Zheng's Blog

My Blog: [jairzh.com](https://www.jairzh.com)

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

