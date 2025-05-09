# texmf

## [`tex/cfont.tex`](tex/cfont.tex)

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
[`tex/cfont.tex`](tex/cfont.tex)
对这两个控制系列做了简单的包装来解决一些不便之处：
- `\jfont` 只会对 `\font` 可处理范围外的字符生效。
	粗略地说，通过 `\jfont` 设置的字体只会对 ascii 字符生效。
	在实际使用上，如果中英文需要同样的字体，
	在通过 `\jfont` 设置后还需要用相同的字体名、尺寸再次调用 `\font`，
	并且设置的切换指令还不能重名
- `\jfont` 和 `\font` 设置的字体尺寸都不可变。
	如果要使用同名字体的不同尺寸，
	就要重新调用完整的 `\jfont` 或 `\font` 并传入相同字体名
	> 这种设计源自 TeX 的美好期望。
	> 实际上 *the TeX book* 中就已经明确提到过
	> " *设计精良的字体应该对应不同的尺寸有不同的图像* " 。
	> 相当于 TeX 认为同一字体的每个不同字号都应该有单独的字体文件，
	> 而不是用同一个字体文件的缩放来变换字号。
	> 可惜人们的精力还不足以实现这么强烈的对美的追求。

因此，[`tex/cfont.tex`](tex/cfont.tex) 实际上就是做了两件事：
- 提供一个指令同时设置中英文字体，并且能用一个字体切换指令同时切换中英文字体
- 将字号和字体分离
	- 设置字号后所有字体设置都会自动使用该字号，除非重新设置字号
	- 重新设置字号后自动应用当前已设置的字体

以上这些基本上都是通过寄存器存取来控制，所以完全遵循分组作用域的限定

### 用法

#### 设置字体文件

- `\afont`  
	设置 ascii 字体
	- `\afont\newfont={[TerminusTTF.ttf]}` 基本等同于
		`\font\newfont={[TerminusTTF.ttf]}`
		- 字体名外的 `{}` 不可省略
		- 不能通过 `\afont` 设置字号，需要另外通过下面提到的其它指令设置
- `\ufont`  
	和 `\afont` 类似，但是调用 `\jfont` 而非 `\font` ，字体名后可以用 `;` 分隔来设置 jfm
	- 用 `\afont` `\ufont` 定义同一个字体切换指令时，例如：
		```latex
		\afont\newfont={[TerminusTTF.ttf]}%
		\jfont\newfont={[LXGWWenKaiGB-Regular.ttf];jfm=ujis}%
		```
		两者不会相互覆盖， `\newfont` 会同时设置这两种字体分别应用于 ascii/unicode
- `\cfont`  
	和 `\afont` `\jfont` 类似，但设置的字体会同时应用于 ascii 字符和 unicode 字符

#### 设置字号

- `\afontsize`  
	改变通过 `\afont` 或 `\cfont` 设置的 ascii 字体大小
	- 用法格式为 `\afontsize{at 12pt}`
		> TeX 有两种字号大小单位， `at` 和 `scaled`，
		> 因此参数中不能只有数值，需要带有 `at` 或 `scaled` 。
- `\ufontsize`  
	和 `\afontsize` 类似，但是用于设置 unicode 字体
- `\fontsize`  
	和 `\afontsize` `\ufontsize` 类似，但是同时作用于 ascii 字体和 unicode 字体

另外 [`tex/cfont.tex`](tex/cfont.tex) 还定义了一些类似 LaTeX 的尺寸。
例如 `\tiny` `\Large`。
但需要注意，这些定义只是尺寸本身，不会设置字体，所以用法会和 LaTeX 不同，
需要配合 `\fontsize` 系列使用才有效，
例如 `\fontsize\tiny`

#### 一些无用的技巧

##### 在分组中定义全局字体

```latex
\input cfont.tex%
\begingroup%
\gdef\ft{\afont\ft={[cmunrm]}\ft}%
\gdef\ft{\cfont\ft={[HaranoAjiGothic-Regular.otf];jfm=jis}\ft}%
\endgroup%
%
%此处 \ft 仍然有效
%此处仍然可以通过 \fontsize 系列指令更改字号
```
