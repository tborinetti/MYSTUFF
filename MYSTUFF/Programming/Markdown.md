---
tags:
  - Programming
---
## Documentation
---
[Markdown Guide](https://www.markdownguide.org/)
[Obsidian Basic Formatting Syntax](https://help.obsidian.md/Editing+and+formatting/Basic+formatting+syntax)
[Obsidian Advanced Formatting Syntax](https://help.obsidian.md/Editing+and+formatting/Advanced+formatting+syntax)
[Mermaid](https://mermaid.js.org/intro/getting-started.html)

## Tags
---
To create a tag, enter a hash symbol (`#`) in the editor, followed by a keyword. For example:
```
#Tags
```

## Headings
---
To create a heading, add up to six `#` symbols before your heading text. The number of `#` symbols determines the size of the heading.
```
# This is a heading 1 
## This is a heading 2 
### This is a heading 3 
#### This is a heading 4 
##### This is a heading 5 
###### This is a heading 6
```

## Horizontal Rules
---
To create a horizontal rule, use three or more asterisks `***`, dashes `---`, or underscores `___` on a line by themselves.

## Links
---
##### Internal Links
Obsidian supports two formats for [internal links](https://help.obsidian.md/Linking+notes+and+files/Internal+links) between notes:
- Linking within Obsidian `[[This Links to an Obsidian File]]`
- Linking within a file `[Heading](#Heading)`
##### External Links
If you want to link to an external URL, you can create an inline link by surrounding the link text in brackets `[ ]`, and then the URL in parentheses `( )`.
```md
[Obsidian Help](https://help.obsidian.md)
```

## Bold, Italics and Highlighting
---

| Style                  | Syntax                 | Example                                  | Output                                 |
| ---------------------- | ---------------------- | ---------------------------------------- | -------------------------------------- |
| Bold                   | `** **` or `__ __`     | `**Bold text**`                          | **Bold text**                          |
| Italic                 | `* *` or `_ _`         | `*Italic text*`                          | _Italic text_                          |
| Bold and nested italic | `** **` and `_ _`      | `**Bold text and _nested italic_ text**` | **Bold text and _nested italic_ text** |
| Bold and italic        | `*** ***` or `___ ___` | `***Bold and italic text***`             | **_Bold and italic text_**             |
| Strikethrough          | `~~ ~~`                | `~~Striked out text~~`                   | ~~Striked out text~~                   |
| Highlight              | `== ==`                | `==Highlighted text==`                   | ==Highlighted text==                   |
|                        |                        |                                          |                                        |

## Obsidian Tips, Notes and Info Blocks
---
> [!tip] Tips can have Custom Titles
> Syntax:
> ```
>  >[!tip] Tips can have Custom Titles
>  >Body of Tips

> [!info]- 
> Tips, Notes and Info Blocks can be collapsible too!
> Syntax:
> `>[!info]-`

> [!note]-
> `>[!note]`-

> [!abstract]-
> `>[!abstract]`-

> [!todo]-
> `>[!todo]`-

> [!question]-
> `>[!question]`-

> [!success]-
> `>[!success]`-

> [!failure]-
> `>[!failure]`-

> [!warning]-
> `>[!warning]`-

> [!danger]-
> `>[!danger]`-

> [!example]-
> `>[!example]`-

> [!bug]-
> `>[!bug]`-

> [!quote]-
> `>[!quote]`-

