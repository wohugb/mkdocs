# 编辑文档

如何布局和编写Markdown源文件。

---

## 文件布局

Your documentation source should be written as regular Markdown files (see
[Writing with Markdown](#writing-with-markdown) below), and placed in the
[documentation directory](configuration.md#docs_dir).
By default, this directory
will be named `docs` and will exist at the top level of your project, alongside
the `mkdocs.yml` configuration file.

The simplest project you can create will look something like this:

```no-highlight
mkdocs.yml
docs/
    index.md
```

By convention your project homepage should always be named `index`.
Any of the
following extensions may be used for your Markdown source files: `markdown`,
`mdown`, `mkdn`, `mkd`, `md`.
All Markdown files included in your documentation
directory will be rendered in the built site regardless of any settings.

You can also create multi-page documentation, by creating several Markdown
files:

```no-highlight
mkdocs.yml
docs/
    index.md
    about.md
    license.md
```

您使用的文件布局确定用于生成的页面的URL。
鉴于上述布局，将为以下URL生成页面：

```no-highlight
/
/about/
/license/
```

如果更适合您的文档布局，您还可以在嵌套目录中包含Markdown文件。

```no-highlight
docs/
    index.md
    user-guide/getting-started.md
    user-guide/configuration-options.md
    license.md
```

嵌套目录中的源文件将导致使用嵌套URL生成页面，如下所示：

```no-highlight
/
/user-guide/getting-started/
/user-guide/configuration-options/
/license/
```

### 索引页面

当请求目录时，默认情况下，大多数Web服务器将返回该目录中包含的索引文件（通常名为`index.html`）（如果存在）。
因此，上面所有示例中的主页都被命名为`index.md`，MkDocs在构建站点时将呈现给`index.html`。

许多存储库托管站点通过在浏览目录内容时显示README文件的内容来为README文件提供特殊处理。
因此，MkDocs允许您将索引页面命名为`README.md`而不是`index.md`。
这样，当用户浏览您的源代码时，存储库主机可以显示该目录的索引页，因为它是README文件。
但是，当MkDocs渲染您的站点时，该文件将重命名为`index.html`，以便服务器将其作为正确的索引文件提供。

如果在同一目录中找到`index.md`文件和`README.md`文件，则使用`index.md`文件并忽略`README.md`文件。

### 配置页面和导航

`mkdocs.yml`文件中的[nav]（configuration.md＃nav）配置设置定义哪些页面包含在全局站点导航菜单中以及该菜单的结构。
如果未提供，将通过发现中的所有Markdown文件自动创建导航[文档目录]（configuration.md #docs_dir）。
自动创建的导航配置将始终按字母数字按字母顺序排序（除了索引文件将始终首先列在子节中）。
如果您希望导航菜单的排序方式不同，则需要手动定义导航配置。

简单的导航配置如下所示:

```no-highlight
nav:
- 'index.md'
- 'about.md'
```

导航配置中的所有路径必须相对于`docs_dir`配置选项。
如果该选项设置为默认值`docs`，则上述配置的源文件将位于`docs / index.md`和`docs / about.md`。

上面的示例将导致在顶层创建两个导航项，并根据文件内容（或文件中未定义标题的文件名）推断其标题。
要为页面定义自定义标题，可以在文件名之前添加标题。

```no-highlight
nav:
- Home: 'index.md'
- About: 'about.md'
```

请注意，如果为导航中的页面定义了标题，则该标题将在整个站点中用于该页面，并将覆盖页面本身中定义的任何标题。

可以通过在节标题下将相关页面列在一起来创建导航子节。

例如：

```no-highlight
nav:
- Home: 'index.md'
- User Guide:
    - 'Writing your docs': 'writing-your-docs.md'
    - 'Styling your docs': 'styling-your-docs.md'
- About:
    - 'License': 'license.md'
    - 'Release Notes': 'release-notes.md'
```

通过上述配置，我们有三个顶级项目：“主页”，“用户指南”和“关于”。
“主页”是指向该网站主页的链接。
在“用户指南”部分下面列出了两个页面：“编写文档”和“样式化文档”。
在“关于”部分下面列出了另外两个页面：“许可证”和“发行说明”。

请注意，部分不能分配页面。
节只是子页面和子节的容器。
您可以根据需要深入嵌套部分。
但是，请注意不要让用户通过过度复杂的嵌套来浏览网站导航太困难。
虽然部分可能会镜像您的目录结构，但它们不必。

导航配置中未列出的任何页面仍将呈现并包含在构建的站点中，但是，它们不会从全局导航链接，也不会包含在“previous”和“next”链接中。
除非直接链接，否则此类页面将被“隐藏”。

## 用Markdown写作

MkDocs pages must be authored in [Markdown][md], a lightweight markup language
which results in easy-to-read, easy-to-write plain text documents that can be
converted to valid HTML documents in a predictable manner.

MkDocs uses the [Python-Markdown] library to render Markdown documents to HTML.
Python-Markdown is almost completely compliant with the [reference
implementation][md], although there are a few very minor [differences].

In addition to the base Markdown [syntax] which is common across all Markdown
implementations, MkDocs includes support for extending the Markdown syntax with
Python-Markdown [extensions].
See the MkDocs' [markdown_extensions]
configuration setting for details on how to enable extensions.

MkDocs includes some extensions by default, which are highlighted below.

[Python-Markdown]: https://python-markdown.github.io/
[md]: https://daringfireball.net/projects/markdown/
[differences]: https://python-markdown.github.io/#differences
[syntax]: https://daringfireball.net/projects/markdown/syntax
[extensions]: https://python-markdown.github.io/extensions/
[markdown_extensions]: configuration.md#markdown_extensions

### 内部链接

MkDocs allows you to interlink your documentation by using regular Markdown
[links].
However, there are a few additional benefits to formatting those links
specifically for MkDocs as outlined below.

[links]: https://daringfireball.net/projects/markdown/syntax#link

#### 链接到页面

When linking between pages in the documentation you can simply use the regular
Markdown [linking][links] syntax, including the *relative path* to the Markdown
document you wish to link to.

```no-highlight
Please see the [project license](license.md) for further details.
```

When the MkDocs build runs, these Markdown links will automatically be
transformed into an HTML hyperlink to the appropriate HTML page.

!!! warning
    Using absolute paths with links is not officially supported.
Relative paths
    are adjusted by MkDocs to ensure they are always relative to the page.
Absolute
    paths are not modified at all.
This means that your links using absolute paths
    might work fine in your local environment but they might break once you deploy
    them to your production server.

If the target documentation file is in another directory you'll need to make
sure to include any relative directory path in the link.

```no-highlight
Please see the [project license](../about/license.md) for further details.
```

The [toc] extension is used by MkDocs to generate an ID for every header in your
Markdown documents.
You can use that ID to link to a section within a target
document by using an anchor link.
The generated HTML will correctly transform
the path portion of the link, and leave the anchor portion intact.

```no-highlight
Please see the [project license](about.md#license) for further details.
```

Note that IDs are created from the text of a header.
All text is converted to
lowercase and any disallowed characters, including white-space, are converted to
dashes.
Consecutive dashes are then reduced to a single dash.

There are a few configuration settings provided by the toc extension which you
can set in your `mkdocs.yml` configuration file to alter the default behavior:

`permalink`:

: Generate permanent links at the end of each header.
Default: `False`.

    When set to True the paragraph symbol (&para; or `&para;`) is used as the
    link text.
When set to a string, the provided string is used as the link
    text.
For example, to use the hash symbol (`#`) instead, do:

        markdown_extensions:
            - toc:
                permalink: "#"

`baselevel`:

: Base level for headers.
Default: `1`.

    This setting allows the header levels to be automatically adjusted to fit
    within the hierarchy of your HTML templates.
For example, if the Markdown
    text for a page should not contain any headers higher than level 2 (`<h2>`),
    do:

        markdown_extensions:
            - toc:
                baselevel: 2

    Then any headers in your document would be increased by 1.
For example, the
    header `# Header` would be rendered as a level 2 header (`<h2>`) in the HTML
    output.

`separator`:

: Word separator.
Default: `-`.

    Character which replaces white-space in generated IDs.
If you prefer
    underscores, then do:

        markdown_extensions:
            - toc:
                separator: "_"

Note that if you would like to define multiple of the above settings, you must
do so under a single `toc` entry in the `markdown_extensions` configuration
option.

```yml
markdown_extensions:
    - toc:
        permalink: "#"
        baselevel: 2
        separator: "_"
```

[toc]: https://python-markdown.github.io/extensions/toc/

#### 链接到图像和媒体

As well as the Markdown source files, you can also include other file types in
your documentation, which will be copied across when generating your
documentation site.
These might include images and other media.

For example, if your project documentation needed to include a [GitHub pages
CNAME file] and a PNG formatted screenshot image then your file layout might
look as follows:

```no-highlight
mkdocs.yml
docs/
    CNAME
    index.md
    about.md
    license.md
    img/
        screenshot.png
```

To include images in your documentation source files, simply use any of the
regular Markdown image syntaxes:

```Markdown
Cupcake indexer is a snazzy new project for indexing small cakes.

![Screenshot](img/screenshot.png)

*Above: Cupcake indexer in progress*
```

Your image will now be embedded when you build the documentation, and should
also be previewed if you're working on the documentation with a Markdown editor.

[GitHub pages CNAME file]: https://help.github.com/articles/using-a-custom-domain-with-github-pages/

#### 从原始HTML链接

Markdown allows document authors to fall back to raw HTML when the Markdown
syntax does not meets the author's needs.
MkDocs does not limit Markdown in this
regard.
However, as all raw HTML is ignored by the Markdown parser, MkDocs is
not able to validate or convert links contained in raw HTML.
When including
internal links within raw HTML, you will need to manually format the link
appropriately for the rendered document.

### Meta-Data

MkDocs includes support for both YAML and MultiMarkdown style meta-data (often
called front-matter).
Meta-data consists of a series of keywords and values
defined at the beginning of a Markdown document, which are stripped from the
document prior to it being processing by Python-Markdown.
The key/value pairs
are passed by MkDocs to the page template.
Therefore, if a theme includes
support, the values of any keys can be displayed on the page or used to control
the page rendering.
See your theme's documentation for information about which
keys may be supported, if any.

In addition to displaying information in a template, MkDocs includes support for
a few predefined meta-data keys which can alter the behavior of MkDocs for that
specific page.
The following keys are supported:

`template`:

: The template to use with the current page.

    By default, MkDocs uses the `main.html` template of a theme to render
    Markdown pages.
You can use the `template` meta-data key to define a
    different template file for that specific page.
The template file must be
    available on the path(s) defined in the theme's environment.

`title`:

: The "title" to use for the document.

    MkDocs will attempt to determine the title of a document in the following
    ways, in order:

    1.
A title defined in the [nav] configuration setting for a document.
    2.
A title defined in the `title` meta-data key of a document.
    3.
A level 1 Markdown header on the first line of the document body.
    4.
The filename of a document.

    Upon finding a title for a page, MkDoc does not continue checking any
    additional sources in the above list.

#### YAML风格元数据

YAML style meta-data consists of [YAML] key/value pairs wrapped in YAML style
deliminators to mark the start and/or end of the meta-data.
The first line of
a document must be `---`.
The meta-data ends at the first line containing an
end deliminator (either `---` or `...`).
The content between the deliminators is
parsed as [YAML].

```no-highlight
---
title: My Document
summary: A brief description of my document.
authors:
    - Waylan Limberg
    - Tom Christie
date: 2018-07-10
some_url: https://example.com
---
This is the first paragraph of the document.
```

YAML is able to detect data types.
Therefore, in the above example, the values
of `title`, `summary` and `some_url` are strings, the value of `authors` is a
list of strings and the value of `date` is a `datetime.date` object.
Note that
the YAML keys are case sensitive and MkDocs expects keys to be all lowercase.
The top level of the YAML must be a collection of key/value pairs, which results
in a Python `dict` being returned.
If any other type is returned or the YAML
parser encounters an error, then MkDocs does not recognize the section as
meta-data, the page's `meta` attribute will be empty, and the section is not
removed from the document.

#### MultiMarkdown样式元数据

MultiMarkdown样式元数据使用[MultiMarkdown]项目首先引入的格式。
该数据由Markdown文档开头定义的一系列关键字和值组成，如下所示：

```no-highlight
Title:   My Document
Summary: A brief description of my document.
Authors: Waylan Limberg
         Tom Christie
Date:    January 23, 2018
blank-value:
some_url: https://example.com

这是该文件的第一段。
```

关键字不区分大小写，可以包含字母，数字，下划线和短划线，并且必须以冒号结尾。
值由行上冒号后面的任何内容组成，甚至可以为空。

如果一行缩进4个或更多空格，则该行被假定为前一个关键字的值的附加行。
关键字可以包含所需的行数。
所有行都连接成一个字符串。

第一个空白行结束文档的所有元数据。
因此，文档的第一行不能为空。

!!! note

    对于MultiMarkdown样式的元数据，MkDocs不支持YAML样式的分隔符（`---`或```）。
    实际上，MkDocs依赖于分隔符的存在与否来确定是否正在使用YAML样式的元数据或MultiMarkdown样式的元数据。
    如果检测到分隔符，但是分隔符之间的内容不是有效的YAML元数据，则MkDocs不会尝试将内容解析为MultiMarkdown样式的元数据。

[YAML]: http://yaml.org
[MultiMarkdown]: http://fletcherpenney.net/MultiMarkdown_Syntax_Guide#metadata
[nav]: configuration.md#nav

### 表

The [tables] extension adds a basic table syntax to Markdown which is popular
across multiple implementations.
The syntax is rather simple and is generally
only useful for simple tabular data.

A simple table looks like this:

```no-highlight
| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |
```

If you wish, you can add a leading and tailing pipe to each line of the table:

```no-highlight
| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |
```

Specify alignment for each column by adding colons to separator lines:

```no-highlight
| First Header | Second Header | Third Header |
| :----------- | :-----------: | -----------: |
| Left         |    Center     |        Right |
| Left         |    Center     |        Right |
```

Note that table cells cannot contain any block level elements and cannot contain
multiple lines of text.
They can, however, include inline Markdown as defined in
Markdown's [syntax] rules.

Additionally, a table must be surrounded by blank lines.
There must be a blank
line before and after the table.

[tables]: https://python-markdown.github.io/extensions/tables/

### 受控代码块

[fenced code blocks]扩展添加了一种定义代码块的替代方法，无需缩进。

第一行应包含3个或更多反引号（````）字符，最后一行应包含相同数量的反引号字符（`````）:

~~~no-highlight
```
Fenced code blocks are like Standard
Markdown’s regular code blocks, except that
they’re not indented and instead rely on
start and end fence lines to delimit the
code block.
```
~~~

使用这种方法，可以选择在反引号后的第一行指定语言，该反引号通知使用的语言的任何语法高亮显示器:

~~~no-highlight
```python
def fn():
    pass
```
~~~

请注意，受限制的代码块不能缩进。因此，它们不能嵌套在列表项，块引用等内。

[fenced code blocks]: https://python-markdown.github.io/extensions/fenced_code_blocks/
