---
weight: 40
bookFlatSection: true
title: "How to build"
---

# How to build cookbook
```bash
git clone https://github.com/Landers1037/cookbook.git
hugo server
# build to html
hugo
```

## With Collapsed
using yaml formatter
```markdown
---
weight: 1
bookCollapseSection: true
title: "This is a title"
---
```

## With TOC
```markdown
---
title: With ToC
weight: 1
---
```

`config.yml` should be like 
```yaml
params:
  # (Optional, default true) Controls table of contents visibility on right side of pages.
  # Start and end levels can be controlled with markup.tableOfContents setting.
  # You can also specify this parameter per page in front matter.
  BookToC: true
```

## Without TOC
```markdown
---
title: Without ToC
weight: 2
bookToc: false
---
```

## Hidden Content
```markdown
---
bookHidden: true
---
```

## ShortCodes
### Section
```html
{{</* section */>}}
```

### Flat Section
```markdown
---
bookFlatSection: true
---
```

### Buttons
```markdown
{{</* button relref="/" [class="..."] */>}}Get Home{{</* /button */>}}
{{</* button href="https://github.com/landers1037" */>}}Contribute{{</* /button */>}}
```
{{< button relref="/" >}}Get Home{{< /button >}}
{{< button href="https://github.com/landers1037" >}}Contribute{{< /button >}}

### Columns
```markdown
{{</* columns */>}} <!-- begin columns block -->
# Left Content
hello, world!

<---> <!-- magic separator, between columns -->

# Mid Content
hello, world!

<---> <!-- magic separator, between columns -->

# Right Content
hello, world!
{{</* /columns */>}}
```
{{< columns >}}

### Details
```markdown
{{</* details "Title" [open] */>}}
## Markdown content
hello, world!
{{</* /details */>}}
```
```markdown
{{</* details title="Title" open=true */>}}
## Markdown content
hello, world!
{{</* /details */>}}
```
{{< details "Title" open >}}
{{< /details>}}

### Expand
```markdown
{{</* expand */>}}
## Markdown content
hello, world!
{{</* /expand */>}}

{{</* expand "Custom Label" "..." */>}}
## Markdown content
hello, world!
{{</* /expand */>}}
```
{{< expand >}}
## Markdown content
hello, world!
{{< /expand >}}

### Hints
```markdown
{{</* hint [info|warning|danger] */>}}
**Markdown content**  
hello, world!
{{</* /hint */>}}
```
{{< hint info >}}
**Markdown content**  
hello, world!
{{< /hint >}}

{{< hint warning >}}
**Markdown content**  
hello, world!
{{< /hint >}}

{{< hint danger >}}
**Markdown content**  
hello, world!
{{< /hint >}}

### Katex
```markdown
{{</* katex [display] [class="text-center"]  */>}}
f(x) = \int_{-\infty}^\infty\hat f(\xi)\,e^{2 \pi i \xi x}\,d\xi
{{</* /katex */>}}
```
{{< katex display >}}
f(x) = \int_{-\infty}^\infty\hat f(\xi)\,e^{2 \pi i \xi x}\,d\xi
{{< /katex >}}

### Mermaid
```markdown
{{</* mermaid [class="text-center"]*/>}}
stateDiagram-v2
    State1: The state with a note
    note right of State1
        Important information! You can write
        notes.
    end note
    State1 --> State2
    note left of State2 : This is the note to the left.
{{</* /mermaid */>}}
```

{{< mermaid >}}
stateDiagram-v2
    State1: The state with a note
    note right of State1
        Important information! You can write
        notes.
    end note
    State1 --> State2
    note left of State2 : This is the note to the left.
{{< /mermaid >}}

### Tabs
```markdown
{{</* tabs "uniqueid" */>}}
{{</* tab "MacOS" */>}} # MacOS Content {{</* /tab */>}}
{{</* tab "Linux" */>}} # Linux Content {{</* /tab */>}}
{{</* tab "Windows" */>}} # Windows Content {{</* /tab */>}}
{{</* /tabs */>}}
```
{{< tabs "uniqueid" >}}
{{< tab "MacOS" >}}MacOS{{< /tab >}}
{{< tab "Linux" >}}Linux{{< /tab >}}
{{< tab "Windows" >}}Windows{{< /tab >}}
{{< /tab >}}