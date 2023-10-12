@def title = "Bugs & etc... e tal"

## Problemas com o Kernel do Julia no Jupyter Notebook

Para resolver problemas com o kernel do Julia no Jupyter instalado em seu computador, você digitar os comandos abaixo no REPL do Julia.

```julia-repl
julia > using Pkg
julia > Pkg.update()
julia > Pkg.build("IJulia")
julia > using IJulia; notebook()
```
Após digitar esses códigos, você deve voltar ao Jupyter e o kernel do Julia deverá estar funcionando normalmente.

