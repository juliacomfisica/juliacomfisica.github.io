@def title = "Notas"
@def hascode = true
@def date = Date(2019, 3, 22)
@def rss = "A short description of the page which would serve as **blurb** in a `RSS` feed; you can use basic markdown here but the whole description string must be a single line (not a multiline string). Like this one for instance. Keep in mind that styling is minimal in RSS so for instance don't expect maths or fancy styling to work; images should be ok though: ![](https://upload.wikimedia.org/wikipedia/en/3/32/Rick_and_Morty_opening_credits.jpeg)"

@def tags = ["syntax", "code"]

# Notas

\toc

## Comunidades

* [Discourse](https://discourse.julialang.org/)

* [Zulip](https://julialang.zulipchat.com/)

* [Julia Forem](https://forem.julialang.org/)

## Download e documentação

* [Download](https://julialang.org/downloads/)
* [Documentação](https://docs.julialang.org/en/v1/)

## Livros

* [Livros](https://julialang.org/learning/books/)

## Problemas com o Kernel do Julia no Jupyter Notebook

Para resolver problemas com o kernel do Julia no Jupyter instalado em seu computador, você digitar os comandos abaixo no REPL do Julia.

```julia-repl
julia > using Pkg
julia > Pkg.update()
julia > Pkg.build("IJulia")
julia > using IJulia; notebook()
```
Após digitar esses códigos, você deve voltar ao Jupyter e o kernel do Julia deverá estar funcionando normalmente.
