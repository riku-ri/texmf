# texmf

## [`tex/page.tex`](tex/page.tex)

一些简单的纸张调整

---

### 用法

- `\pageL`  
	调整纸张左右宽度和左边距
	- `\pageL{500pt}{2em}` 将纸张左右宽度调整为 500pt 并将左边距调整为 2em
		- 如果第 1 个参数为空例如 `\pageL{}{2em}` 将只设置左边距而不改变纸张左右宽度
		- 如果第 2 个参数为空例如 `\pageL{500pt}{}` 将只设置纸张左右宽度而不改变左边距
- `\pageR`  
	与 `\pageL` 类似但调整右边距
- `\pageLR`  
	与 `\pageL` `\pageR` 类似但同时调整左右边距
- `\pageT`  
	与 `\pageL` 类似但调整的是纸张上下长度和顶部边距
- `\pageB`  
	与 `\pageL` 类似但调整的是纸张上下长度和底部边距
- `\pageTB`  
	与 `\pageLR` 但同时调整的是纸张上下长度和上下边距
