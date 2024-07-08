Draft slides for SMBE 2024.

To build:

```sh
pandoc -t beamer impg_prez.md -o impg_prez.pdf --pdf-engine=xelatex -V theme:metropolis -V aspectratio:169 -V mainfont="Arial" -H metropolis-bw.sty
```
