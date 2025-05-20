# texmf

## [`tex/lfont.tex`](tex/lfont.tex)

A simple wrapper of luatexja.

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

- `\afontsize`  
	改变通过 `\afont` 或 `\lfont` 设置的 ascii 字体大小
	- 用法格式为 `\afontsize{at 12pt}`
		> TeX 有两种字号大小单位， `at` 和 `scaled`，
		> 因此参数中不能只有数值，需要带有 `at` 或 `scaled` 。
- `\ufontsize`  
	和 `\afontsize` 类似，但是用于设置 unicode 字体
- `\fontsize`  
	和 `\afontsize` `\ufontsize` 类似，但是同时作用于 ascii 字体和 unicode 字体

另外 [`tex/lfont.tex`](tex/lfont.tex) 还定义了一些类似 LaTeX 的尺寸。
例如 `\tiny` `\Large`。
但需要注意，这些定义只是尺寸本身，不会设置字体，所以用法会和 LaTeX 不同，
需要配合 `\fontsize` 系列使用才有效，
例如 `\fontsize\tiny`

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
