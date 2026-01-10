# texmf

## [`tex/vlines.tex`](tex/vlines.tex)

Generate fixed width for vertical lines

### Usage

- `\vlines`  
  - Take all continuous groups after it, and each group is a line
    - *e.g.* `\vlines{l1}{l2} others` will get 2 lines `l1` and `l2`,
      `others` is out of range
    - Since all continuous groups will be taken,
      if you need to add another group after all lines,
      insert a `\relax` or any other non-group element:
      ```latex
      \vlines{l1}{l2}\relax {non-line group}
      ```
  - Generate a hbox contain multiple lines,
    the box width is max width amoung all lines

### Example

```latex
\input vlines.tex

\vrule\vbox{here\par there}\vrule

\vskip2em\relax

\vrule\vlines{here}{there}\vrule

\bye
```

- The `\vrule` shows the left and right border of
  `\vbox` and `\vlines` in above code.
  - `\vbox` will generate a page-width box
  - `\vlines` will generate a 5ex-width box
    - 5ex is max width among `here` and `there`
