@def title = "Notas"
@def hascode = true
@def rss = "A short description of the page which would serve as **blurb** in a `RSS` feed; you can use basic markdown here but the whole description string must be a single line (not a multiline string). Like this one for instance. Keep in mind that styling is minimal in RSS so for instance don't expect maths or fancy styling to work; images should be ok though: ![](https://upload.wikimedia.org/wikipedia/en/b/b0/Rick_and_Morty_characters.jpg)"
@def rss_title = "More goodies"
@def rss_pubdate = Date(2019, 5, 1)

@def tags = ["syntax", "code", "image"]


# Blog

\toc

## Animações em Julia com o `Javis.jl`

Seguindo este tutorial passo a passo você obterá, ao final, a seguinte animação:

@@row
@@left ![](/assets/orbita3.gif) @@
~~~
<div style="clear: both"></div>
~~~
@@

Abaixo, apresentamos o código para a animação acima. Mas, antes de dar um **Ctrl+C**, **CtrL+V** para um ambiente Julia, como o Jupyter por exemplo, vamos entender os principais pontos do código?

```
using Javis

function ground(args...)
    background("black") 
    sethue("black") 
end

function object(p=O, color="black")
    sethue(color)
    circle(p, 25, :fill)
    return p
end

function path!(points, pos, color)
    sethue(color)
    push!(points, pos) 
    circle.(points, 2, :fill)
end

myvideo = Video(700, 700)

path_of_blue = Point[]
path_of_gray = Point[]

Background(1:500, ground)
yellow_ball = Object(1:500, (args...)->object(O, "yellow"), Point(0,0))
blue_ball = Object(1:500, (args...)->object(O, "blue"), Point(200,0))
act!(blue_ball, Action(anim_rotate_around(2π, O)))
gray_ball = Object(1:500, (args...)->object(O, "gray"), Point(150,30))
act!(gray_ball, Action(anim_rotate_around(24π, blue_ball)))
Object(1:500, (args...)->path!(path_of_blue, pos(blue_ball), "blue"))
Object(1:500, (args...)->path!(path_of_gray, pos(gray_ball), "gray"))

render(myvideo; pathname="orbitas.gif")
```
Então, bora lá!
 
 O primeiro passo é fazer a instalação do pacote[^1] `Javis.jl`. Se você estiver usando o Jupyter insira os comandos:

```
using Pkg
Pkg.add("Javis")
```
> **Nota 1:** Este passo não consta no código acima. Ele será necessário apenas na primeira vez que você for usar o pacote em seu computador.

Após a instalação precisamos carregar o pacote para o nosso código. Para isso, digite o comando abaixo:

```
using Javis
```
Pronto! Com o pacote instalado e devidamente carregado, podemos iniciar a construção da nossa animação.

Nossa primeira tarefa na construção da animação é definir a função[^2] `ground`. Ela definirá o fundo da animação (_background_) e a cor da "caneta" (_pen_) como preto. Observe o código abaixo:

```
function ground(args...)
    background("black") # cor de fundo da tela.
    sethue("black") # cor da caneta
end
```
Você pode estar se perguntado porque `args...`  na primeira linha. Vamos te explicar. Cada função definida pelo usuário recebe três argumentos: `video` (estrutura do vídeo), `object` (estrutura do objeto) e `frame` (o número de quadros atuais) fornecidos pelo `Javis.jl`.\\

Embora esses argumentos sejam irrelevantes para a função `ground`, precisamos escrever `args...`para que Julia saiba que temos uma função que aceita esses três argumentos. Quanto ao `...` significa basicamente quantos argumentos você quiser.\\

A seguir criamos a função `object`, ela define uma cor para os círculos e os desenha adequadamente. Veja o código abaixo:

```
function object(p=O, color="black")
    sethue(color)
    circle(p, 25, :fill)
    return p
end
```
Na função acima `25` é o raio da bola, tente modificá-lo para ver o que acontece e `:fill` preenche a bola com a cor que definirmos.\\

A última função que criaremos em nosso código é a função `path!`. Ela desenha a trajetória das bolas. Veja o código abaixo:

```
function path!(points, pos, color)
    sethue(color)
    push!(points, pos) # adiciona pos aos pontos
    circle.(points, 2, :fill) # desenha círculos para cada ponto usando broadcasting
end
```
> **Nota 2:** O `pos` assume a **pos**ição de uma determinada bola e passa-o como argumento para a função `path!`.

> **Nota 3:** O conceito de broadcasting está descrito em detalhes no capítulo 7 do livro `Julia com Física: Uma Introdução`.

A seguir criamos a variável `myvideo` e atribuímos a ela um `video`, ele define a tela de vídeo para a animação. Aqui nossa tea possui 700 pixels de largura por 700 pixels de altura. 

```
myvideo = Video(700, 700) # 700 x 700 (largura x altura)

```
Cada objeto ou ação que criamos durante o código é adicionado ao `myvideo`.


A seguir criamos duas variáveis `path_of_blue` e `path_of_gray` que armazenarão em um vetor [^3] os pontos da trajetória das bolas azul e cinza. 

```
path_of_blue = Point[]
path_of_gray = Point[]
```
Logo após,  usamos `Background` para especificar que a função `ground` será aplicada a todos os objetos descritos posteriormente.
```
Background(1:500, ground)
```
o valor $1:500$, se refere a cada quadro da animação de 1 a 500 e está relacionado também a veocidade da animação. Varie o valor 500 para mais e para menos para ver o que acontece. _Vamos dar spoiler: Quando diminuímos a taxa para, por exemplo, 1:200 a animação fica mais rápida_. 

Abaixo descrevemos o código para a `yellow_ball`. Ela representa o Sol e estará localizada no centro da animação e não possuirá movimento.

```
yellow_ball = Object((args...)->object(O, "yellow"), Point(0,0))
```
Posteriormente definimos o objeto `blue_ball`, que representará o planeta Terra em nossa animação.

```
blue_ball = Object(1:500, (args...)->object(O, "blue"), Point(200,0))
act!(blue_ball, Action(anim_rotate_around(2π, O)))
```
Perceba que a cor e a posição da bola `blue_ball` são definidos na função anônima [^4] `(args...)->object(O, "blue"), Point(200,0))`. Onde `Point(200,0)` significa que `blue_ball` estará localizada à 200 pixels da origem na direção x.\\

Logo abaixo de `blue_ball` aparece pela primeira vez o método `act!`. 

O trecho de código`act!(blue_ball, Action(anim_rotate_around(2π, O)))` fará com que `blue_ball`gire em um ângulo de $2π$ em torno do centro da animação.

Abaixo definimos o `gray_ball`, ela representará a Lua em nossa simulação.

```
gray_ball = Object(1:500, (args...)->object(O, "gray"), Point(150,30))
act!(gray_ball, Action(anim_rotate_around(24π, blue_ball)))
```
Perceba que assim como ocorreu com `blue_ball` a cor e a posição de `gray_ball` também são definidos na função anônima `(args...)->object(O, "gray"), Point(150,30))`. Onde  `Point(150,30)` significa que `gray_ball` estará localizada à 150 pixels da origem na direção x e 30 pixels da origem na direção y.\\

O trecho de código `act!(gray_ball, Action(anim_rotate_around(24π, blue_ball)))` diz que `gray_ball` irá girar em um ângulo de  $24π$ em torno de `blue_ball`. Isso fará com que nossa Lua descreva uma espécie de espiral para contornar a Terra. Tente modificar esse valor para ver o que acontece.

A seguir vemos que os objetos `path_of_blue`e `path_of_gray`(caminho de `blue_ball` e caminho de`gray_ball`) são definidos nas funções anônimas `(args...)->path!(path_of_blue, pos(blue_ball), "blue")` e `(args...)->path!(path_of_gray, pos(gray_ball), "gray")` respectivamente. Isso desenhará a trajetória de `blue_ball` e de `gray_ball`.
```
Object(1:500, (args...)->path!(path_of_blue, pos(blue_ball), "blue"))
Object(1:500, (args...)->path!(path_of_gray, pos(gray_ball), "gray"))
```
Faça um teste. Apague ou comente (basta colocar # antes do código) a função `path!` e estas duas inhas acima para ver o que acontece. _Spoiler: Não haverá mais o desenho da trajetória em nossa simulação._ Va em frente e teste!

Finalmnte, o comando `render`, renderiza todos os objetos definidos no `video` que definimos como `myvideo` e o produz como um arquivo `gif`.
```
render(myvideo; pathname="orbitas.gif")
```

Agora sim, copie e cole o código em um ambiente Julia e o modifique quantas vezes quiser e aprecie o resultados.

[^1]: O processo de instalação de pacotes é descrito com detalhes no capítluo 3 do livro `Julia com Física: Uma Introdução`.
[^2]: O conceito de função e sua sintaxe são descritos no capítulo 6  do livro `Julia com Física: Uma Introdução`.
[^3]: O conceito de arrays (vetores e matrizes) é explicado em detalhe no capítulo 4 do livro `Julia com Física: Uma Introdução`.
[^4]: O conceito de função anônima é explicado no capítulo 6  do livro `Julia com Física: Uma Introdução`. 

* **O pacote foi desenvolvido por:**

[Ole Kröger](https://github.com/Wikunia)\\
[Jacob Zelko](https://gist.github.com/TheCedarPrince)

* **Referência**

[1] [Javis.Jl](https://wikunia.github.io/Javis.jl/stable/)


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

* Rádio 

A busca pelo elemento rádio pode se dar por: `elements["radium"]`, `elements[:Ra]` ou por `elements[88]`, veja o código abaixo.

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
* Polônio

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

* Desenvolvedores do `PeriodicTable.jl`

[Steven G. Johnson](https://github.com/stevengj)\\
[Jacob Wikmark](https://github.com/lancebeet)\\
[Carsten Bauer](https://github.com/crstnbr)\\

* Referência

[1] [Conheça as mulheres (esquecidas) por trás da tabela periódica](https://revistagalileu.globo.com/Ciencia/noticia/2019/03/conheca-mulheres-esquecidas-por-tras-da-tabela-periodica.html)

[2] [8 Elementos químicos invisíveis da tabela periódica](https://super.abril.com.br/blog/superlistas/8-elementos-quimicos-incriveis-da-tabela-periodica/)

[3] [Mundo Educação - Marie Curie](https://mundoeducacao.uol.com.br/quimica/marie-curie.htm#:~:text=Em%201898%2C%20Marie%20anunciou%20%C3%A0,Nobel%20em%20%C3%A1reas%20cient%C3%ADficas%20diferentes.
)

[4] [JuliaPhysics - PeriodicTable.jl](https://github.com/JuliaPhysics/PeriodicTable.jl)


