@def title = "Bugs, Etc & tal"

# Bugs, Etc & tal

\toc
## Bugs

# Problemas com o Kernel do Julia no Jupyter Notebook

Para resolver problemas com o kernel do Julia no Jupyter instalado em seu computador, você digitar os comandos abaixo no REPL do Julia.

```julia-repl
julia > using Pkg
julia > Pkg.update()
julia > Pkg.build("IJulia")
julia > using IJulia; notebook()
```
Após digitar esses códigos, você deve voltar ao Jupyter e o kernel do Julia deverá estar funcionando normalmente.

## Citando

Araújo, F.A.G; Oliveira, M.M.. Julia com física: uma introdução. 1. ed. Fortaleza: SEDUC-CE, 2023. 114 p.

## Primeira versão de *Julia com Física: uma introdução* - Draft

Draft - [Julia com Física: uma introdução](https://drive.google.com/file/d/1J30tFO4b3_au5MbKr7Fn92EYhTIkybVV/view?usp=sharing).

## Contato

E-mail: juliacomfisica@gmail.com
