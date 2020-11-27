@def title = "Notas"
@def hascode = true
@def rss = "A short description of the page which would serve as **blurb** in a `RSS` feed; you can use basic markdown here but the whole description string must be a single line (not a multiline string). Like this one for instance. Keep in mind that styling is minimal in RSS so for instance don't expect maths or fancy styling to work; images should be ok though: ![](https://upload.wikimedia.org/wikipedia/en/b/b0/Rick_and_Morty_characters.jpg)"
@def rss_title = "More goodies"
@def rss_pubdate = Date(2019, 5, 1)

@def tags = ["syntax", "code", "image"]


# Notas

\toc

## Tabela Periódica 
Em 2019 a tabela periódica completou 150 anos. Sua primeira versão, concebida, em 1789, por Antoine Lavoisier possuía 33 elementos químicos. Hoje, ela conta com 119 elementos, organizados sistematicamente de acordo com seus números atômicos, camadas eletrônicas, número de elétrons na camada de valência e propriedades físicas.\\
Vários cientista contribuíram para o desenvolvimento dela, dentre eles se destaca a Física Marie Curie. Ela foi a primeira mulher a ganhar o prêmio Nobel e a primeira pessoa e única mulher a ganhá-lo por duas vezes. Um em Física ao demonstrar a existência da radioatividade natural em 1903 e outro em 1911, em Química, pela descoberta de dois novos elementos: o rádio e o polônio.

Em Julia há um pacote que nos permite acessar os elementos da tabela periódica, chama-se `PeriodicTable.jl`. Primeiramente precisamos instalá-lo. Para isso, basta digitar no REPL do Julia o código a seguir e pressionar Enter.

```julia-repl
] add PeriodicTable

```
Caso você esteja usando o Jupyter, o procedimento é o mesmo que descrevemos na seção 3.1 do livro `Julia com Física: Uma Introdução`, quando falamos de instalação de pacotes. Digite:\\

```
using Pkg
Pkg.add("PeriodicTable")
```

Os procedimentos que descreveremos a partir de agora é o mesmo para ambos os ambientes.\\

Para obter acesso ao banco de dados do `PeriodicTable.jl`contendo os elementos da tabela periódica, digitamos o seguinte trecho de código:

```julia-repl
julia> using PeriodicTable

julia> elements
Elements(…119 elements…):
H                                                  He
Li Be                               B  C  N  O  F  Ne
Na Mg                               Al Si P  S  Cl Ar
K  Ca Sc Ti V  Cr Mn Fe Co Ni Cu Zn Ga Ge As Se Br Kr
Rb Sr Y  Zr Nb Mo Tc Ru Rh Pd Ag Cd In Sn Sb Te I  Xe
Cs Ba    Hf Ta W  Re Os Ir Pt Au Hg Tl Pb Bi Po At Rn
Fr Ra    Rf Db Sg Bh Hs Mt Ds Rg Cn Nh Fl Mc Lv Ts Og
Uue                                                   
      La Ce Pr Nd Pm Sm Eu Gd Tb Dy Ho Er Tm Yb Lu    
      Ac Th Pa U  Np Pu Am Cm Bk Cf Es Fm Md No Lr
```

É possível procurar pelos elementos através de três formas. Como exemplo usaremos os elementos descobertos por Marie Skłodowska Curie: O rádio e o Polônio. 

### Rádio 

A busca pelo elemento rádio pode se dar por: `elements["radium"]`, `elements[:Ra]` ou por n `elements[88]`, veja o código abaixo.

```julia-repl
julia> elements["radium"]
Radium (Ra), number 88:
        category: alkaline earth metal
     atomic mass: 226.0 u
         density: 5.5 g/cm³
   melting point: 1233.0 K
   boiling point: 2010.0 K
           phase: Solid
          shells: [2, 8, 18, 32, 18, 8, 2]
e⁻-configuration: 1s² 2s² 2p⁶ 3s² 3p⁶ 4s² 3d¹⁰ 4p⁶ 5s² 4d¹⁰ 5p⁶ 6s² 4f¹⁴ 5d¹⁰ 6p⁶ 7s²
      appearance: silvery white metallic
         summary: Radium is a chemical element with symbol Ra and atomic number 88. It is the sixth element in group 2 of the periodic table, also known as the alkaline earth metals. Pure radium is almost colorless, but it readily combines with nitrogen (rather than oxygen) on exposure to air, forming a black surface layer of radium nitride (Ra3N2).
   discovered by: Pierre Curie
          source: https://en.wikipedia.org/wiki/Radium
```
### Polônio

A busca pelo elemento polônio se dá da mesma maneira que fizemos anteriormente: `elements["polonium"]`, `elements[:Po]` ou  `elements[84]`.

```julia-repl
julia> elements["polonium"]
Polonium (Po), number 84:
        category: post-transition metal
     atomic mass: 209.0 u
         density: 9.196 g/cm³
      molar heat: 26.4 J/mol⋅K
   melting point: 527.0 K
   boiling point: 1235.0 K
           phase: Solid
          shells: [2, 8, 18, 32, 18, 6]
e⁻-configuration: 1s² 2s² 2p⁶ 3s² 3p⁶ 4s² 3d¹⁰ 4p⁶ 5s² 4d¹⁰ 5p⁶ 6s² 4f¹⁴ 5d¹⁰ 6p⁴
      appearance: silvery
         summary: Polonium is a chemical element with symbol Po and atomic number 84, discovered in 1898 by Marie Curie and Pierre Curie. A rare and highly radioactive element with no stable isotopes, polonium is chemically similar to bismuth and tellurium, and it occurs in uranium ores. Applications of polonium are few.
   discovered by: Pierre Curie
          source: https://en.wikipedia.org/wiki/Polonium
```
Para cada elemento é listado: Categoria, massa atômica, densidade, ponto de fusão e ebulição, por quem foi descoberto, dentre outras características. Segundo seus desenvolvedores algumas informações foram (e serão) adicionadas ao longo do tempo.

### Desenvolvedores do `PeriodicTable.jl`

[Steven G. Johnson](https://github.com/stevengj)\\
[Jacob Wikmark](https://github.com/lancebeet)\\
[Carsten Bauer](https://github.com/crstnbr)\\

### Referência

[1] [Conheça as mulheres (esquecidas) por trás da tabela periódica](https://revistagalileu.globo.com/Ciencia/noticia/2019/03/conheca-mulheres-esquecidas-por-tras-da-tabela-periodica.html)

[2] [8 Elementos químicos invisíveis da tabela periódica](https://super.abril.com.br/blog/superlistas/8-elementos-quimicos-incriveis-da-tabela-periodica/)

[3] [Mundo Educação - Marie Curie](https://mundoeducacao.uol.com.br/quimica/marie-curie.htm#:~:text=Em%201898%2C%20Marie%20anunciou%20%C3%A0,Nobel%20em%20%C3%A1reas%20cient%C3%ADficas%20diferentes.
)

[4] [JuliaPhysics - PeriodicTable.jl](https://github.com/JuliaPhysics/PeriodicTable.jl)

## Problemas com o Kernel do Julia no Jupyter Notebook

Para resolver problemas com o kernel do Julia no Jupyter instalado em seu computador, você digitar os comandos abaixo no REPL do Julia.

```julia-repl
julia > using Pkg
julia > Pkg.update()
julia > Pkg.build("IJulia")
julia > using IJulia; notebook()
```
Após digitar esses códigos, você deve voltar ao Jupyter e o kernel do Julia deverá estar funcionando normalmente.
