---
title: Markdown
category: Markup
layout: 2017/sheet
prism_languages: [markdown]
updated: 2017-09-20
weight: -1
---

## Reference
{:.-three-column}

### Headers

```markdown
# h1
### h3
```

```markdown
Header 1
========
```

```markdown
Header 2
--------
```

### Emphasis

```markdown
*italic*
_italic_
```

```markdown
**bold**
__bold__
```

```markdown
`code`
```

### Links

```markdown
[link](http://google.com)
```

```markdown
[link][google]
[google]: http://google.com
```

```markdown
<http://google.com>
```

### Images

```markdown
![Image alt text](/path/to/img.jpg)
![Image alt text](/path/to/img.jpg "title")
![Image alt text][img]
```

```markdown
[img]: http://foo.com/img.jpg
```

### Code

```
    4 space indent
    makes a code block
```

~~~markdown
```
code fences
```
~~~


~~~markdown
```js
codeFences.withLanguage()
```
~~~

### Blockquotes

```markdown
> This is
> a blockquote
>
> > Nested
> > Blockquote
```

### Horizontal line

```markdown
----
```

```markdown
****
```
