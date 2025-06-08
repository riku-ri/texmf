# texmf

## [`tex/align.tex`](tex/align.tex)

一些方便制作对齐阵列的内容

### 用法

- `\chalign`  
	`\chalign{#1}{#2}`将得到一个`\haling{#1#2}`且垂直居中的盒子，例如：
	```
	\chalign{$#$&\quad\hss#\hss\cr}{here&there}
	```
