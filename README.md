# Internationalized Reactive MarkDown

Turbocharge square brackets with this flavor of MarkDown. Frameworks may grant them access to everything a reactive template can access. Provide a MarkDown file in multiple languages to make the experience internationalized. These changes are likely to cause undesired results when using other MarkDown flavors. A careless conversion can lead to a different semantic meaning or errors.

> [!IMPORTANT]  
> Extensive incompatibilities with CommonMark, GitHub-flavored MarkDown and many more.

## Structure

Most lines in the following example are named after their purpose.

<pre>Title <span title="First line is title if not heading or code.">⚠️</span>

Description ...

#keyword
@author
[en-US]
2024-01-01
[canonical](https://github.com/jogemu/irmd)

```yaml
metadata: yes
``` <span title="Conditional body start after code.">⚠️</span>

---

# Heading <span title="Conditional body start before heading.">⚠️</span>

[Summary](details:Details)

* [Description List](dd:Contains groups of terms and their descriptions.)

[CSS](abbr:Cascading Style Sheet)

[Referenced]([^1])[^2]

[1]: https://github.com "GitHub" @github @microsoft 2024</a>
[2]:
Footnotes may span

over multiple lines
until double newline


@mention
#hashtag
</pre>

Although a file is valid on its own, it is recommended to store it in a folder to better accommodate multiple languages and shared logic. This relationship can be made explicit by using a valid [RFC 5646] language as the filename.

[RFC 5646]: https://datatracker.ietf.org/doc/html/rfc5646

```
📁 project name
├─ 📁 file name
│  ├─ 📄 en.md
│  ├─ 📄 de.md
│  └─ 📄 fr.md
└─ 📁 component name
   ├─ 📄 en-US.md
   ├─ 📄 data.json
   ├─ 📄 angular.js
   ├─ 📄 react.js
   └─ 📄 vue.js
```

## Fundamentals

The fundament of this flavor is the syntax `[🟥](🟢:🔷)`, whose short form `[🟥](🔷)` is equivalent to how other flavors allow links and images. In this case `🟢:` corresponds to `https:` or `file:` when omitted. 🟥🟢🔷 may contain newlines only if newlines are present directly after every opening and directly before every closing bracket/parentheses that encloses newlines. Unbalanced brackets/parentheses must be escaped with a backslash that is itself unescaped.

If `[🟥]` is not followed by `(` it is equivalent to above if `🟥` is defined as `🟢:🔷` by the framework or in the markdown file. The latter is achieved using `[🟥]:(🟢:🔷)` surrounded by newlines. It is allowed to omit `(` and `)` if `🟢:🔷` does not contain consecutive newlines. No omission of otherwise necessary newlines. Thus, the shortest form `[🟥]: 🔷` is equivalent to [shortcut reference links]. However, the syntax for [full reference links] changed to `[💬]([🟥])`.

[shortcut reference links]: https://spec.commonmark.org/0.31.2/#shortcut-reference-link
[full reference links]: https://spec.commonmark.org/0.31.2/#full-reference-link

Full references are merely the introduction to nesting. 🟥🔷 can contain any combination of the variations introduced above. Since HTML only supports one `href` attribute per link, the meaning of additional values must be defined. Either explicitly with 🟢 or by recognizing patterns such as standardized timestamps or email addresses. This provides an easy way to add bibliographic information to a source, similar to how [info strings] provide a title.

[info strings]: https://spec.commonmark.org/0.31.2/#info-string

If 🔷 starts with an url to media such as images, videos, audio and documents then it can be embedded using `![🟥]` or `![🟥](🟢:🔷)`. Specify the media type with 🟢 if the file extension at the end of the url is unreliable. Compared to HTML, controls are not hidden by default. The behavior is different for non-media. Exclaimed HTML details/dialog elements are opened and abbreviations like `![🟥](abbr:🔷)` are `🔷 (💬)` instead of just `💬`, where 💬 is `<abbr title="🔷">🟥</abbr>`. 

The plural of abbreviations and i18n properties is available using `[🟥]s` to get `💬s` and `![🟥]s` to get `🔷s (💬s)`. `[^🟥]` links to the first embed of 🟥, 🔷 in the printed bibliography or 🟥 as a footnote. A single line of a file or a single page of a can be referenced using `[^🟥#L2]` or `[^🟥#page=2]`.

## Slimmer

Building on this, the following short forms are translated into the foundation.

| Meaning  | Shortened | `[🟥](🟢:🔷)` |
| -------- | --------- | ------ |
| *Italic* | `➡️*🆑*➡️` | `[🆑](em:)`     |
| **Bold** | `➡️**🆑**➡️` | `[🆑](strong:)` |
| `Code`   | <code>\`🆑\`</code> | `[🆑](code:)`   |
| Heading  | `↩️🔁# 🆑` | `[🆑](h🔢:)`    |
| <blockquote>Blockquote</blockquote> | `↩️> 🆑`    | `[🆑](blockquote:)` |
| <pre>Block</pre> | <pre>↩️\`\`\`🟢<br>↩️🟥<br>↩️\`\`\`</pre>    | <pre>[<br>🟥<br>]\(🟢:)</pre> |
| <ul><li>List</li></ul><ol><li>List</li></ol><dl><dt>Summary</dt><dd>Definition</dd></dl> | `↩️* 🆑`<br>`↩️🔢. 🆑`<br>`↩️* [🟥](dd:🔷)` | `[](ul:[🆑])`<br>`[](ol:[🆑])`<br>`[](dl:{🟥:🔷})` |
| Person | `➡️🔤@🔡➡️`<br>`➡️@🔤➡️`<br>ORCID | `[🔤](mailto:🔤@🔡)` |
| Time | `➡️🕝➡️` | `[🕝](time:🕝)` |
| <sub>Subscript</sub> | `$_🆑$` | `[🆑](sub:)` |
| <sup>Superscript</sup> | `$^🆑$` | `[🆑](sup:)` |
| <kbd>Keyboard</kbd> | `[⌨️🆑]` | `[🆑](kbd:)` |
| <mark>Marked</mark> | `[🟨🆑]` | `[🆑](mark:)` |

|    | definition |
| -- | ------------ |
| ➡️ | space, start or end of line |
| 🆑 | 🟥 without leading/trailing spaces and newlines |
| ↩️ | newline followed by no spaces |
| 🔁 | repeat character to the right 0 to ∞ times |
| 🔢 | placeholder for one number |
| 🔤 | String without spaces, unescaped @ and newlines |
| 🔡 | String without spaces, unescaped @ and newlines |
| 🕝 | date, time, both or duration |

The head goes until before the first heading or after the first code block, whichever comes first. It starts at the beginning of the file and can be empty or contain the entire file. Put metadata and information usually found on the front matter there.

## Default attributes

| MD            | HTML                   |
| ------------- | ---------------------- |
| `[🟥](a:🔷)` | `<a href="🔷">🟥</a>` |
| `[🟥](abbr:🔷)`<br>`[🟥](abbr:🔷)s`<br>`![🟥](abbr:🔷)`<br>`![🟥](abbr:🔷)s` | `<abbr title="🔷">🟥</abbr>`<br>`<abbr title="🔷">🟥</abbr>s`<br>`🔷 (<abbr title="🔷">🟥</abbr>)`<br>`🔷s (<abbr title="🔷">🟥</abbr>s)` |
| `[🟥](audio:🔷)`<br>`![🟥](audio:🔷)` | `<a href="🔷">🟥</a>`<br>`<audio controls src="🔷">🟥</audio>` |
| `[🟥](base:🔷)` | `<base href="🔷"/>` |
| `[🟥](bdo:🔷)` | `<bdo dir="🔷">🟥</bdo>` |
| `[🟥](blockquote:🔷)` | `<blockquote cite="🔷">🟥</blockquote>` |
| `[🟥](button:🔷)` | `<button type="🔷">🟥</button>` |
| `[🟥](col:🔷)`        | `<col span="🔷"/>` |
| `[🟥](colgroup:🔷)`   | `<colgroup span="🔷">🟥</colgroup>` |
| `[🟥](dd:🔷)`         | `<dl><dt>🟥</dt><dd>🔷</dd></dl>` |
| `[🟥](del:🔷 🕝)` | `<del cite="🔷" datetime="🕝">🟥</del>` |
| `[🟥](details:🔷)`<br>`![🟥](details:🔷)`    | `<details><summary>🟥</summary>🔷</details>`<br>`<details open><summary>🟥</summary>🔷</details>` |
| `[🟥](dfn:🔷)`        | `<dfn title="🔷">🟥</dfn>` |
| `[🟥](dialog:🔷)`<br>`![🟥](dialog:🔷)` | `<dialog>🟥</dialog>`<br>`<dialog open>🟥</dialog>` |
| `[🟥](embed:🔷)`<br>`![🟥](embed:🔷)` | `<a href="🔷">🟥</a>`<br>`<embed src="🔷"/>` |
| `[🟥](fieldset:🔷)`   | `<fieldset><legend>🟥</legend>🔷</fieldset>` |
| `[🟥](figure:🔷)`     | `<figure><figcaption>🟥</figcaption>🔷</figure>` |
| `[🟥](form:🔷)`       | `<form method="🔷">🟥</form>` |
| `[🟥](iframe:🔷)`<br>`![🟥](iframe:🔷)` | `<a href="🔷">🟥</a>`<br>`<iframe src="🔷">🟥</iframe>` |
| `[🟥](img:🔷)`        | `<img src="🔷" alt="🟥"/>` |
| `[🟥](input:🔷)`      | `<label><span>🟥</span><input value="🔷"/></label>` |
| `[🟥](ins:🔷 🕝)` | `<ins cite="🔷" datetime="🕝">🟥</ins>` |
| `[🟥](label:🔷)`      | `<label for="🔷">🟥</label>` |
| `[🟥](link:🔷)`       | `<link href="🔷" rel="🟥"/>` |
| `[🟥](meta:🔷)`       | `<meta name="🔷" content="🟥"/>` |
| `[🟥](meter:🔷)`      | `<label><span>🟥</span><meter value="🔷"></meter></label>` |
| `[🟥](picture:🔷)`<br>`![🟥](picture:🔷)` | `<a href="🔷">🟥</a>`<br>`<picture><source srcset="🔷"/>🟥</picture>` |
| `[🟥](progress:🔷)`   | `<label><span>🟥</span><progress value="🔷"></progress></label>` |
| `[🟥](q:🔷)`          | `<q cite="🔷">🟥</q>` |
| `[🟥](ruby:🔷)`       | `<ruby>🟥<rp>(</rp><rt>🔷</rt><rp>)</rp></ruby>` |
| `[🟥](select:🔷)`     | `<label><span>🟥</span><select>🔷</select></label>` |
| `[🟥](textarea:🔷)`   | `<label><span>🟥</span><textarea>🔷</textarea></label>` |
| `[🟥](time:🕝)`       | `<time datetime="🕝">🟥</time>` |
| `[🟥](track:🔷)`      | `<track src="🔷"/>` |
| `[🟥](video:🔷)`      | `<video controls><source src="🔷"/>🟥</video>` |
