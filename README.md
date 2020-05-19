# Random Stuff

This repository contains random stuff from my daily routine.

## Markdown to pdf conversion

Using [pandoc] we can convert any valid markdown documents into pdf documents.
A sample command for generating...

```bash
pandoc --pdf-engine=xelatex -V colorlinks \
                            -V urlcolor=NavyBlue \
                            -V toccolor=Red \
                            --highlight-style zenburn \
                            homebrew_previous_opencv.md \
                            -o output.pdf
```

Some very detailed notes on the usage of `pandoc` is available here [markdown to pdf] and [learnbyexample]


[pandoc]: https://pandoc.org

[markdown to pdf]: https://jdhao.github.io/2019/05/30/markdown2pdf_pandoc/

[learnbyexample]: https://learnbyexample.github.io/tutorial/ebook-generation/customizing-pandoc/
