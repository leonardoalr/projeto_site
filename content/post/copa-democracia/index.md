---
date: "2023-01-15"
image:
  caption: 'Image credit: **Leonardo Rodrigues**'
  placement: 0.3
math: true
title: Copa da Democracia - plano de aula utilizando o R
---
### Plano de Aula
**Duração**: 2 aulas de 1h e 40 minutos

### Contexto

Durante a copa, trombei com esse tuíte do professor Chris Hanretty:

{{< tweet 1594315070284398592 >}}

Na sequência do tuíte, o professor apresenta a classificação dos grupos da copa de acordo com o nível de democracia dos países. Achei que o exercício encaixava bem na temática de **"Poder e Democracia"**, prevista para o mês de dezembro na disciplina de Sociologia.

Adaptei sua ideia e apliquei o seguinte plano de aula.

### Visão Geral

O/a estudante irá aprofundar nas principais características do sistema democrático por meio de um exercício prático. Para isso, será utilizado um índice que mensura o nível de democracia em diferentes países.

Os países selecionados foram aqueles que participaram da **Copa do Mundo de Futebol de 2022** e foram classificados de acordo com os 8 grupos da competição. O produto desse exercício é a classificação dos países em seus grupos de acordo com seu nível de democracia. Um exemplo de resultado é o gráfico a seguir:

{{< chart data="line-chart" >}}

### Organização

Esse plano está dividido em duas seções. Na primeira seção, busca-se responder duas perguntas: **quais são as características de uma democracia?** **Como medir a democracia em determinado país?** Na segunda seção, faremos a aplicação prática desse índice. Isso poderá ser feito de duas maneiras: através da linguagem R, se houver disponibilidade de computadores para os estudantes; de forma manual, se não houver interesse na aplicação da linguagem ou ausência de computadores (no final deste post, apresendo mais detalhes desse formato).

### Utilizando o R em sala de aula

Eu apliquei esse plano para as turmas de 3º ano. O plano funcionou tanto para alunos com nenhuma ou pouca familiaridade com linguagens de programação, quanto para estudantes do curso técnico em Informática. No entanto, foi preciso algumas adaptações para os primeiros. A aplicação se deu de forma exclusivamente instrumental (estudantes não participaram da construção do script, apenas rodaram o código).

Para professores/as e estudantes sem familiaridade com a linguagem R, a aplicação manual também pode ser uma alternativa.

### Seção 1
Duração: 1h 40 minutos

## Passo 1

**Apresentação do problema**

Iniciei com a seguinte apresentação: a Seleção Brasileira ganhou de 2 a 0 da Sérvia no jogo de estreia na Copa do Mundo. **Mas, quem ganharia a partida se a disputa fosse por “democracia” e não por gols?**

Escolhi a Sérvia como adversária apenas porque era o jogo mais recente quando apliquei esse plano. Nesse momento, pedi aos estudantes que palpitassem sobre o resultado e deixei que eles discutissem alguns minutos sobre as justificativas de seus palpites. Também, anotei no quadro alguns dos principais indicadores de democracia que apareceram nas falas dos estudantes e que coincidiram com aqueles que serão apresentados no passo seguinte.

## Passo 2

**O que é considerado democracia?**

Para construir o placar entre Brasil e Sérvia teríamos que criar uma medida. Para o futebol, o “gol” é o conceito por trás do 2 a 0. Mas, e na democracia? Discuti os princípios para a definição de democracia apresentados pelo [V-DEM](https://www.v-dem.net/data/reference-documents/).

De acordo com o V-DEM, dificilmente teremos um consenso sobre o que é democracia além de uma vaga definição de **“poder do povo”**. Mas, alguns princípios democráticos sempre aparecem nos debates sobre se há ou não democracia em determinada região. São eles:

- **Eleitoral**: fazer com que os governantes sejam responsivos aos eleitores com eleições periódicas.
- **Liberal**: princípio que trata da proteção dos direitos dos indivíduos e das minorias contra a “tirania da maioria” e a repressão do Estado. Na prática: liberdades civis protegidas pela constituição e pesos e contrapesos que limitam o poder.
- **Majoritária**: princípio que afirma que a maioria das pessoas deve ser capacitada a governar e implementar seus desejos através da política.
- **Consensual**: princípio que enfatiza que a maioria não pode desconsiderar as minorias e que há um valor inerente na representação de grupos com interesse e visões divergentes.
- **Participativa**: trata da participação direta dos cidadãos em todo o processo político. É levado em conta participações civis além da eleitoral ou votos diretos.
- **Deliberativa**: a busca pelo bem público deve ser informado por um processo de diálogo respeitoso e racional em todos os níveis; ao invés de ser via coerção ou apelos sentimentais.
- **Igualitária**: desigualdades materiais dificultam o exercício de direitos e das liberdades públicas. Mais desigualdade, menor participação.

Essas definições são produtos de uma tradução rápida daquelas apresentadas pelo V-DEM. Aqui, vale a apresentação de exemplos cotidianos para ilustrar cada um dos princípios. Na minha experiência, é comum notar a surpresa dos estudantes ao conhecer as diversas "dimensões" da democracia para além daquelas diretamente ligadas a eleições.

## Passo 3

**Qual princípio utilizar? Como mensurar esse princípio?**

Nesse ponto, é preciso destacar que iremos medir apenas 1 desses princípios. O objetivo é exemplificar a complexidade que é o exercício de construção dos índices e as etapas pelo qual eles passam. Primeiro, fizemos uma abordagem conceitual. Agora, um esforço de aplicação.

De forma arbitrária, vamos escolher o princípio de **democracia eleitoral**. Para medi-lo, tal qual o V-DEM, será utilizado um outro conceito, o de **Poliarquia**, de **Robert Dahl** (indique aos alunos que deem uma olhadinha no [verbete do autor](https://pt.wikipedia.org/wiki/Robert_Dahl) no Wikipedia e, também, no verbete sobre [Poliarquia](https://pt.wikipedia.org/wiki/Poliarquia)). Aqui, vale destacar que uma vantagem em utilizar o conceito de poliarquia é que podemos considerar diferentes níveis de democratização. É isso que estamos buscando ao comparar Brasil e Sérvia. Outras abordagens podem ser absolutas demais (há ou não democracia) e, em vários casos, pode esconder nuances entre diferentes contextos.

**E quais são os requisitos para uma democracia e como mensurar esses níveis de democracia?**

Os requisitos são:

- **Eleições livres, justas e frequentes** _vamos chamar de "Componente 1" (c1_)
- **Dirigentes eleitos** _vamos chamar de c2_
- **Liberdade de expressão** _vamos chamar de c3_
- **Fontes alternativas de informação** _também vamos chamar de c3_
- **Cidadania inclusiva** ou **sufrágio** _vamos chamar de c4_
- **Autonomia de associação** _vamos chamar de c5_

Nesse ponto, destaquei que cada um desses requisitos tem um índice que varia entre 0 e 1. Como medir cada componente? Alguns são mais fáceis do que outros. O _C4_ por exemplo é medido pelo % da população com sufrágio (possibilidade de participar do jogo eleitoral). No Brasil, toda a população pode votar (com exceção de crianças). Por isso, seu valor é o máximo (1,0).

Outros, são mais complicadinhos. O _C2_, por exemplo é composto por 19 outros índices. São considerados, por exemplo, o número de câmaras, o percentual da câmara menor que é eleita diretamente, o percentual da câmara maior que é eleita diretamente, e etc. Para cada um desses índices ver no [apêndice A do Codebook](https://www.v-dem.net/documents/1/codebookv12.pdf).

É importante mencionar o caráter coletivo do trabalho acadêmico e seus bastidores. Lembrar aos estudantes que, para o nosso exercício, teríamos que coletar toda essa informação para TODOS os componentes tanto no Brasil quanto na Sérvia. Teríamos que coletar fontes nos dois países, ler documentos oficiais, entrevistar pessoas, codificar essas informações, padronizá-las... enfim, um trabalho impossível de se fazer em pouco tempo, com poucos recursos e poucas pessoas.

É um trabalho impossível de se fazer sozinho ou entre as pessoas participantes dessa aula! Nossa sorte é que alguém já fez isso. Um grupo de pessoas já coletou esses dados e padronizou todos os índices em uma escala de 0 a 1. Aqui, nós já vamos pegar os índices prontos.

## Passo 4

**Agora, os procedimentos.**

Acrescentei na lousa, na frente de cada requisito, o índice para o Brasil e para a Sérvia no formato de uma tabela.

Esses índices estão disponíveis no banco de dados do V-DEM. Para facilitar, o banco de dados simplificado e em formato fácil de abrir em Excel está disponível no link abaixo.

{{% staticref "uploads/democracia_simplificado.xlsx" "newtab" %}}AQUI{{% /staticref %}} para fazer o download da planilha.

{{< table path="results.csv" header="true" caption="Tabela 1: Índices de cada componente por país" >}}

Bom, e agora? Como criar um índice a partir desses diferentes componentes? Bastaria somar para saber quem tem mais pontos? É uma alternativa. Vamos fazer!


{{< math >}}
$$\text{Soma dos componentes}=\frac{1}{8}\text{c1}+\frac{1}{8}\text{c2}+\frac{1}{4}\text{c3}+\frac{1}{8}\text{c4}+\frac{1}{4}\text{c5}$$
{{< /math >}}

Repare que se trata de uma média ponderada. Os pesquisadores identificaram que _c2_ e _c4_ poderiam ser facilmente incluídos em alguns países apenas formalmente. Em outras palavras, para medir a democracia, eles consideram que os demais índices deveriam ter peso maior do que _dirigentes eleitos_ e _cidadania inclusiva_. Não porque são princípios menores do que os outros, mas porque são mensurados de forma diferente.

No entanto, a soma ou uma média dos componentes pode ignorar algumas ausências. Por exemplo, a Sérvia apresenta 100% no componente de dirigentes eleitos ( _c2_ ), mas apresenta apenas 0,353 em eleições livres, limpas e justas ( _c1_ ). Em um caso hipotético, do que adiantaria os dirigentes serem eleitos se apenas uma parcela minúscula da população tivesse votado? Ao somar corremos o risco de ignorar componentes fundamentais da democracia.

Como driblar esse problema? Precisamos penalizar aqueles países que tem baixos índices em alguns componentes fundamentais. A solução matemática para isso é a multiplicação. Para o caso do Brasil, o baixo índice em c3 será multiplicado por todos os outros índices. Em outras palavras, um componente com índice baixo penalizará todos os outros altos. A solução seria:


{{< math >}}
$$\text{Multiplicação dos componentes}=\text{c1}\times\text{c2}\times\text{c3}\times\text{c4}\times\text{c5}$$
{{< /math >}}

No entanto, na multiplicação também teremos um ônus. Por exemplo, um país pode ter uma sociedade civil tão participativa que poderia compensar a inexistência de dirigentes eleitos diretamente. Por isso, para chegar ao meio termo entre a multiplicação e a soma, a alternativa encontrada foi fazer uma média dos dois índices. A fórmula final é:

{{< math >}}
$$\text{Poliarquia} = \frac{\text{Soma dos Componentes}+\text{Multiplicação dos componentes}}{2}$$
{{< /math >}}

Aqui, dei um tempo para que os estudantes fizessem a conta e depois compartilhassem o resultado.

**O resultado entre Brasil e Sérvia foi**:
- Índice de Poliarquia no Brasil: 0.661
- Índice de Poliarquia na Sérvia: 0.336

Por fim, comparei os resultados com os palpites dos estudantes. Nesse caso, a maior parte dos estudantes foi surpreendida com o índice da Sérvia. Nos minutos finais, discutimos os principais pontos de "vantagem" do Brasil e sugeri uma pesquisa sobre o histórico do atual governo sérvio.

### Seção 2
Duração: 1h 40 minutos

## Aplicação do Índice para a Copa da Democracia

A aplicação do índice para a **Copa da Democracia** foi feita através da _versão 1_ apresentada abaixo. Para isso, escrevi o código linha por linha junto com os estudantes explicando cada uma das funções e seus objetivos. Fizemos o gráfico apresentado no início desse post. 

Depois, dividi a turma em 7 grupos. Cada grupo ficou responsável por analisar um grupo da Copa do Mundo. Aqui, os estudantes tiveram que adaptar o script para a sua demanda específica.

No final, os estudantes apresentaram o gráfico de seu grupo para a turma e discutimos as principais surpresas ou resultados esperados.

## Versão 1

**Utilização do R para estudantes habituados com linguagem de programação**

Nessa versão, eu adaptei o script criado pelo professor Chris Hanretty para uma versão mais simplificada. Por isso, os gráficos também são mais simples. A simplificação do script foi uma estratégia para aproximar os estudantes da linguagem, para que eles tivessem o maior controle sobre cada uma das linhas. Além disso, não utilizei o banco de dados completo do V-DEM. No campus, temos problemas com internet e optei por diminuir o tamanho do banco de dados para agilizar o download. Para facilitar a explicação, dei novos nomes às variáveis originais. Você pode baixar o banco simplificado no link abaixo.

{{% staticref "uploads/democracia.csv" "newtab" %}}Link para o banco de dados{{% /staticref %}}


*Código*

```r
#Pacotes necessários
library(tidyverse)
library(hrbrthemes)

#Carregar banco de dados simplificado
dados <- read.csv("democracia.csv")

#### Vamos fazer por grupo da copa ####
#Selecionando grupo A
grp_g <- c("BRA", "SRB", "CHE", "CMR")

#Filtrando pelo grupo selecionado 
meu_grupo <- dados %>% filter(sigla %in% grp_a)

#### Gráfico ####
#Organizar o banco em ordem de índice
meu_grupo <- meu_grupo %>%
  arrange(v2x_polyarchy) %>%
  mutate(country_name = fct_inorder(nome))

# Gráfico #
ggplot(meu_grupo, aes(x = country_name,
                      y = v2x_polyarchy)) +
  scale_x_discrete("") +
  scale_y_continuous("Índice de Democracia Eleitoral [0-1]",
                     limits = c(0, 1)) + 
  geom_point(shape = 21, size = 4, col = "#df4ca3", fill = "#df4ca3") +
  labs(title = "Grupo A",
       subtitle = "Nível de democracia eleitoral",
       caption = "Dados do Projeto V-DEM. Gráficos inspirados em: @chrishanretty") + 
  coord_flip() + 
  theme_ipsum_tw(base_size = 14)  +
  theme(plot.title.position = "plot",
        plot.caption.position =  "plot")
        

#para selecionar outros grupos você poderá utilizar as seguintes siglas
#grp_a <- c("QAT", "ECU", "SEN", "NLD")
#grp_b <- c("ENG", "IRN", "USA", "WAL")
#grp_c <- c("ARG", "SAU", "MEX", "POL")
#grp_d <- c("FRA", "AUS", "DNK", "TUN")
#grp_e <- c("ESP", "CRI", "DEU", "JPN")
#grp_f <- c("BEL", "CAN", "MAR", "HRV")
#grp_g <- c("BRA", "SRB", "CHE", "CMR")
#grp_h <- c("PRT", "GHA", "URY", "KOR")

```

O resultado desse código é o gráfico apresentado acima.


## Versão 2

**Utilização do R para estudantes sem familiaridade com linguagem de programação**

O plano B para essa aula seria a simples reprodução do script pelo professor e a distribuição dos resultados entre os estudantes. Eu utilizaria essa estratégia caso não fosse possível utilizar o laboratório de informática. O objetivo seria utilizar o script como um exemplo de prática acadêmica para a análise de contextos democráticos. Assim, pularíamos a escrita do código e faríamos apenas a divisão dos grupos e análise dos resultados.

O código original, criado pelo professor Chris Hanretty, pode ser encontrado em seu Github ([clique aqui](https://gist.github.com/chrishanretty/7faabc9bdc3ed3c611efe63137ac9b30)).

## Versão 3

**Na mão!**

Uma alternativa, _que ainda não testei_, é a utilização do formato manual. Nesse caso, o professor pode distribuir uma planilha com os índices dos países e os estudantes poderiam fazer o cálculo do índice manualmente e/ou o desenho do gráfico. Abaixo, disponibilizo a planilha que você poderá abrir no Excel.

{{% staticref "uploads/democracia_simplificado.xlsx" "newtab" %}}AQUI{{% /staticref %}} para fazer o download da planilha.

## Comentários
- O banco de dados do V-DEM é muito interessante. Explorá-lo pode trazer mais ideias de como abordar o tema de democracia com exemplos práticos.
- Pontos positivos dessa abordagem: recebi feedbacks positivos dos estudantes quando a fórmula final do índice foi apresentada. Apresentar os limites da adição e da multiplicação foi uma estratégia bem sucedida. Além disso, a visualização dos gráficos chamou bastante atenção das turmas.
- Pontos negativos: a segunda seção foi bastante cansativa para muitos estudantes. Talvez ela poderia ter sido desmembrada em duas partes.
- Feedbacks de possíveis leitores são MUITO BEM VINDOS.

## Formalidades

**Competências específicas da BNCC contempladas**:
5. Reconhecer e combater as diversas formas de desigualdade e violência, adotando princípios éticos, democráticos, inclusivos e solidários, e respeitando os Direitos Humanos.
6. Participar, pessoal e coletivamente, do debate público de forma consciente e qualificada, respeitando diferentes posições, com vistas a possibilitar escolhas alinhadas ao exercício da cidadania e ao seu projeto de vida, com liberdade, autonomia, consciência crítica e responsabilidade.

**Interdisciplinaridade**: História, Sociologia, Matemática, Educação Física, Geografia, Programação, Banco de dados, por exemplo.

**Materiais**:
- Banco de dados e documentos do [projeto V-DEM](https://www.v-dem.net/data/reference-documents/).
- Tuíte de Chris Hanretty e seu [GitHub](https://gist.github.com/chrishanretty/7faabc9bdc3ed3c611efe63137ac9b30).
