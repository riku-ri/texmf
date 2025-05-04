# texmf

## [`tex/cfont.tex`](tex/cfont.tex)

A simple wrapper of luatexja.

目前 luatex 的中文实现采用 luatexja ，
luatexja 提供了 `\jfont` 设置字体，
`\jfont` 基本上和 `\font` 保持一致的格式和效果，
但是通过 `jfont` 设置的字体只对非 ASCII 字符生效。

基于 `\jfont` `\font` 已经能够处理绝大多数字体操作,
[`tex/cfont.tex`](tex/cfont.tex) 主要解决一些不方便的地方：
- 中英文需要相同的字体时需要既执行 `\jfont` 又执行 `\font`
- 字体尺寸会绑定到字体指令上，例如 `\jfont\newfont=[HaranoAjiGothic-Regular.otf] at 10pt`
	后 `\newfont` 会始终为 `10pt` 大小，
	改变字体大小需要重新执行完整的 `\jfont` 指令

### 用法

- `\afont`
	- 例如 `\afont\newfont=[HaranoAjiGothic-Regular.otf]|{12pt}`
		- 效果基本上等同于 `\font\newfont=[HaranoAjiGothic-Regular.otf] at 12pt`，
			但会设置一些寄存器以便同时设置中英字体、更改字体大小等
	- 字体尺寸外的括号 `{}` 不可省略
	- 由于 luatex `\font` 本身的实现问题，`\newfont` 只对 ASCII 字符生效
- `\ufont`
	- 格式、用法、效果基本等同于 `\afont` ，
		但是内部是用 `\jfont` 设置 unicode 字体而不是 `\font`
- `\cfont`
	- 格式、用法、效果基本等同于 `\afont` `\ufont` ，
		但是对所有字符都生效，不区分 ASCII/unicode
- `\afontsize`
	- 例如 `\afontsize{20pt}` 会保持当前字体同时调整大小为 `20pt`
	- 字体尺寸外的括号 `{}` 不可省略
	- 如果之前没有通过 `\afont` 或 `\cfont` 设置过 ASCII 字体，会在 log 中提示并且无其它效果
- `\ufontsize`
	- 例如 `\ufontsize{20pt}` 会保持当前字体同时调整大小为 `20pt`
	- 字体尺寸外的括号 `{}` 不可省略
	- 如果之前没有通过 `\ufont` 或 `\cfont` 设置过 unicode 字体，会在 log 中提示并且无其它效果
- `\fontsize`
	- 例如 `\fontsize{20pt}` 会保持当前字体同时调整大小为 `20pt`
	- 会同时尝试调整 ASCII 和 unicode 字体，
		ASCII/unicode 中的任一类未设置过时会在 log 中提示并对该类字体无效
- 定义了一些类似 LaTeX 的尺寸，例如 `\tiny`
	- 这些尺寸定义是纯数值，和 LaTeX 中的用法不同，
		需要配合 fontsize 系列指令使用，例如 `\ufontsize\tiny`
