# texmf

TeX will auto load the `.tex` file in the path `$HOME/texmf`,
this repository provide some TeX package.

> 绝大多数情况下，相关说明中提到的 “指令” 都指 TeX 的控制系列(*Control Sequences*)

- [`tex/ifinput.tex`](`tex/ifinput.tex`)
	> \input file.tex;
	- A simple macro to avoid multiple input the same file
- [`tex/lfont.tex`](tex/lfont.tex)
	> [lfont.tex.md](lfont.tex.md)
	- A simple wrapper of luatexja
- [`tex/page.tex`](tex/page.tex)
	> [page.tex.md](page.tex.md)
	- 一些简单的纸张调整
- [`tex/cn-ujis.tex`](tex/cn-ujis.tex)
	> [cn-ujis.tex.md](cn-ujis.tex.md)
	- 一些中文排版的配置
- [`tex/cn`](tex/cn)
	> [cn.md](cn.md)
	- 将其它表记转换为中文
- [`tex/align.tex`](tex/align.tex)
	> [align.tex.md](align.tex.md)
	- 一些方便制作对齐阵列的内容
- [`tex/math.tex`](tex/math.tex)
	> [math.tex.md](math.tex.md)
	- 一些数学排版相关内容
- [`tex/scalemath.tex`](tex/scalemath.tex)
	- 同样与数学相关的内容，但使用了 tikz 因此与上一项分开
	- 目前只是一个简单的示例，
		将`$`内完全的数学内容作为参数传给`\bigvmath`即可
