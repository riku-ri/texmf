# texmf

## [`tex/math.tex`](tex/math.tex)

一些数学排版相关内容

### 分界符

一般来说，`\left`和`\right`只能用于单行算式，
[`tex/math.tex`](tex/math.tex)
定义了`\(`和`\)`来对多行算式做更好的分界符构建。

#### 用法

- 只适用于数学环境中使用`\openup1\jot\halign`自由排版的多行算式
	- 其它方面与`\left`、`\right`完全相同，
		只是做了一些box,glue的计算减去`\left` `right`多余的距离计算。
		对于单行算式而言，
		这些减去的距离计算结果为0,
		因此`\(`、`\)`也适用于单行算式。
