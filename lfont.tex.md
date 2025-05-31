# texmf

## [`tex/lfont.tex`](tex/lfont.tex)

A simple wrapper of luatexja.

---

> 下文中的指令说的都是 TeX 的控制系列(*Control Sequences*)

TeX 主要通过 `\font` 加载和选择字体，
但它无法正确映射字体中的非 ascii 字符，
对于我来说主要是 CJK 字符。
这个问题一般在对应 TeX implementation 中用额外的中文实现解决。
在目前主流的 TeX implementation ，
也就是 luatex 中，中文处理完全使用日本开发者做的 CJK 实现，
即 luatexja 。

luatexja 提供了 `\jfont` 加载和选择字体，
它的格式用法与 `\font` 完全相同，
`\jfont` 和 `\font` 两者已经能够提供完整的字体处理。

但无论是 `\font` 还是 `\jfont` ，
字体本身都是包含字号的，
同一字体的不同字号总是需要重新运行完整的指令来重新加载。
并且对于一个字体设置指令，例如 `\font\ft={[cmunrm]}` 后的 `\ft` ，
再将 `\ft` 用于 `\jfont` 定义，会覆盖 `\font` 的效果。
也就是说同一个自定义指令不能同时用于中英字体。

因此 [`tex/lfont.tex`](tex/lfont.tex) 实际上就是解决了两个问题：
- 将字号和字体分离，可以单独设置字号并自动应用之前的字体文件
- 让一个自定义指令能够同时用于中英字体

中英字体是我作为汉语母语者的通俗说法，
[`tex/lfont.tex`](tex/lfont.tex) 中主要使用的概念其实是
Latin 字体（`\lfont`）和非Laint字体（`\Lfont`）。

### 用法

#### 设置字体文件

- `\lfont`  
	设置 Latin 字体
	- `\afont\newfont={[TerminusTTF.ttf]}` 基本等同于
		`\font\newfont={[TerminusTTF.ttf]}`
		- 字体名外的 `{}` 不可省略
		- 不能通过 `\lfont` 设置字号，需要另外通过下面提到的其它指令设置
- `\Lfont`  
	和 `\lfont` 类似，但是调用 `\jfont` 而非 `\font` ，字体名后可以用 `;` 分隔来设置 jfm
	- 用 `\lfont` `\Lfont` 定义同一个字体切换指令时，例如：
		```latex
		\lfont\newfont={[TerminusTTF.ttf]}%
		\Lfont\newfont={[LXGWWenKaiGB-Regular.ttf];jfm=ujis}%
		```
		两者不会相互覆盖， `\newfont` 会同时设置这两种字体分别应用于 ascii/unicode
- `\lLfont`  
	和 `\lfont` `\Lfont` 类似，但设置的字体会同时应用于 Latin 字符和非 Latin 字符

#### 设置字号

- `\lfontsize`  
	改变通过 `\afont` 或 `\lfont` 设置的 ascii 字体大小
	- 用法格式为 `\lfontsize{at 12pt}`
		> TeX 有两种字号大小单位， `at` 和 `scaled`，
		> 因此参数中不能只有数值，需要带有 `at` 或 `scaled` 。
- `\Lfontsize`  
	和 `\lfontsize` 类似，但是用于设置 unicode 字体
- `\fontsize`  
	和 `\lfontsize` `\Lfontsize` 类似，但是同时作用于 ascii 字体和 unicode 字体

另外 [`tex/lfont.tex`](tex/lfont.tex) 还定义了一些类似 LaTeX 的尺寸。
例如 `\tiny` `\Large`。
但需要注意，这些定义只是尺寸本身，不会设置字体，所以用法会和 LaTeX 不同，
需要配合 `\fontsize` 系列使用才有效，
例如 `\fontsize\tiny`

#### 数学字体

- Latin
	- `\ltextfont`  
		与 `\textfont` 用法类似，
		但 `\ltextfont<fam>=` 后必须紧跟由 `\lfont` 定义的字体指令，
		而**不能**是 `\font` 定义的指令
	- `\lscriptfont`  
		与 `\ltextfont` 类似，但设置 scriptfont 而非 textfont
	- `\lscriptscriptfont`  
		与 `\ltextfont` 类似，但设置 scriptscriptfont 而非 textfont
- 非 Latin
	- `\Ltextfont`  
		与 `\ltextfont` 类似，但是设置非 Latin 字体
	- `\Lscriptfont`  
		与 `\lscriptfont` 类似，但是设置非 Latin 字体
	- `\Lscriptscriptfont`  
		与 `\lscriptscriptfont` 类似，但是设置非 Latin 字体

##### 关于数学字体，不得不说的事项

众所周知， TeX 在数学模式中的排版控制和其它模式有着巨大的区别。
字体就是其中之一。
例如作为上标、下标的字体会自动缩小，
字体本身需要额外绘制用于数学表达式的符号等等。
对于后者，往往需要特别制作的数学字体，
在我的工作环境（Arch Linux, wayland）中，
这些字体会被 `pacman -Fl texlive-basic | grep cmsy | head` 列出:

<details>
<summary>点击此处展开列出的数学字体</summary>

> ```
> texlive-basic usr/share/texmf-dist/fonts/afm/public/amsfonts/cm/cmsy10.afm
> texlive-basic usr/share/texmf-dist/fonts/afm/public/amsfonts/cm/cmsy5.afm
> texlive-basic usr/share/texmf-dist/fonts/afm/public/amsfonts/cm/cmsy6.afm
> texlive-basic usr/share/texmf-dist/fonts/afm/public/amsfonts/cm/cmsy7.afm
> texlive-basic usr/share/texmf-dist/fonts/afm/public/amsfonts/cm/cmsy8.afm
> texlive-basic usr/share/texmf-dist/fonts/afm/public/amsfonts/cm/cmsy9.afm
> texlive-basic usr/share/texmf-dist/fonts/pk/ljfour/public/cm/dpi600/cmsy10.pk
> texlive-basic usr/share/texmf-dist/fonts/pk/ljfour/public/cm/dpi600/cmsy7.pk
> texlive-basic usr/share/texmf-dist/fonts/source/public/cm/cmsy10.mf
> texlive-basic usr/share/texmf-dist/fonts/source/public/cm/cmsy5.mf
> ```

</details><br/>

关于上面所说的“特殊制作”，其中一点便是这些字体的字符映射会和一般的字体不同。
例如 `R` 字符的 ASCII 编码是 `82` ，
因此通用的字体会在内部“第 82 个”字符的位置上保存 `R` 的绘制曲线。
而“特殊制作”的数学字体有可能会绘制数学符号。
相当于在使用这些字体后，在键盘上按下 `R` 后出现的是一个数学符号，
而不是 `R` 这个字母。
> 并非只有数学字体会这么做，许多只显示表情、线条的字体也会如此。

这些行为发生在 TeX 的第2、第3族。
因此一般不建议第2、第3族的字体，
除非你很清楚它们会像上面描述的那样工作。

另外，
luatexja 本身并不支持数学环境下的 CJK 上下标。
因此即使设置了 `\Lscriptfont` 也无法使用 `$anyway^{失望的是}$` 。
目前而言， `\Lscriptfont` 和 `\Lscriptscriptfont` 其实是*无用*的指令。

---
---

数学环境下的排版包含着数不清的细节，
实际上，在数学环境中，我建议始终设置并使用最初的、和谐的非 Latin 字体，
除此之外不再改动。
到了不得不让它更大时，将整个数学盒子放大会是更好的选择。

---
---

#### 一些无用的技巧

##### 在分组中定义全局字体

```latex
\input lfont.tex%
\begingroup%
\gdef\ft{\lfont\ft={[cmunrm]}\ft}%
\gdef\ft{\Lfont\ft={[HaranoAjiGothic-Regular.otf];jfm=jis}\ft}%
\endgroup%
%
%此处 \ft 仍然有效
%此处仍然可以通过 \fontsize 系列指令更改字号
%不过上面的\Lfont覆盖了\lfont，这是正常的，同时设置需要使用\lLfont
```
