# texmf

## [`tex/math.tex`](tex/math.tex)

一些数学排版相关内容

<!--

### 分界符

一般来说，`\left`和`\right`只能用于单行算式，
[`tex/math.tex`](tex/math.tex)
定义了`\mlt`和`\mrt`来对多行算式做更好的分界符构建。

- 在陈列模式下
	- 其它方面与`\left`、`\right`完全相同，
		只是做了一些box,glue的计算减去`\left` `right`多余的距离计算。
		对于单行算式而言，
		这些减去的距离计算结果为0,
		因此`\(`、`\)`也适用于单行算式
- 在行内数学模式下，
	`\mlt`和`\mrt`会在算式本身高度就小于计算结果时不做改变。
	因此也适用于行内数学模式

-->

### 算式标记

标记与跳转是超文本常用的功能，
现在 pdf 本身也支持文件本身包含标记与跳转。

[`tex/math.tex`](tex/math.tex)
提供了一个标记与引用（而非跳转）的指令
- `\mformula#1`
	- 如果`\mformular#1`是：
		- 第一次出现在文档中  
			- 编号加一，并用增加后的编号标记此处的算式
				- 编号是对于整个文档的、全局的、单个数值
			- 在算式右方一个`\quad`后排版一个编号
				- 由括号和数值组成，例如 `(1)` `(2)`
				- 此处排版的编号本身在一个无宽度的水平盒子中，
					所以在此编号后同行的内容又会回退到算式正后方排版
					> 所以**它不适用于行内编号**。
					> 如果需要用`\mformular`对算式进行编号和引用，
					> 即使它很短，也应放在陈列排版中并独占一行
		- 已经出现过
			- 排版一个编号，只是普通的`1` `2` 等字符。只有数字而无括号

例如
```latex
$$\displaylines{
\left|\cal R\right|\not=\left|\aleph\right|\mformula{inf0}\cr
\left|\cal R\right|\not=\left|\aleph\right|\mformula{inf0}\cr
\left|\cal R\right|\not=\left|\aleph\right|\mformula{inf1}\cr
}$$

\par 此处引用\mformula{inf0}，但没有什么特别的意义
\par 此处引用了另一个\mformula{inf1}，但也没有什么特别的意义
\par 此处引用了另一个(\mformula{inf1})，它展示了引用时需要另外加上括号，
而标记时只能固定使用带括号的数字
```

将排版三行算式，其中第一行和第三行看起来比较和谐，
但第二行会有奇怪的缩进并且没有括号。
这是因为第二行用了重复的`{inf0}`，
对于`\mformula`，这不是在对算式编号，而是紧随其后引用了另一个算式的标记。

接下来又会在新行中排版：
```
此处引用1，但没有什么特别的意义
此处引用了另一个2，但也没有什么特别的意义
此处引用了另一个(2)，它展示了引用时需要另外加上括号，而标记时只能固定使用带括号的数字
```
