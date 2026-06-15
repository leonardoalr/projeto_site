---
title: "Exercício de raspagem dos dados do Twitter"
image:
  caption: " "
  placement: 0.3
date: "2022-06-30"
math: true
---

### Como raspar dados do Twitter?

**Atualização**: com as mudanças na plataforma, o exercício e os scripts estão desatualizados.

### Contexto

Esse exercício tem o objetivo de apresentar as possibilidades de uso do **R** para o acompanhamento de reações nas redes sociais em eventos ao vivo. A escolha de uma partida do **Galo** foi aleatória e nada tem a ver com as preferências pessoais do autor. Você poderá aplicar o mesmo exercício para análises em campanhas políticas, por exemplo.

Utilizei como referência a análise que Bruno Vidigal fez para a Premier League. [Veja aqui](https://www.brunovidigal.com/posts/2021-01-10-premier-league-twitter-data-visualization-with-r/).

### Como acompanhar as reações?

Primeiro, coletamos todos os tuítes que citavam a palavra "Galo" ou "Emelec". Defini 10 mil tuítes que, por tentativa e erro, cobriram o período de tempo entre o pré-jogo e a hora da minha coleta.

```r
dados <- search_tweets(q = 'GALO OR EMELEC',
                       n = 100000,
                       include_rts = FALSE,
                       retryonratelimit = TRUE)
```

Depois, selecionei apenas os tuítes que ocorreram entre o apito inicial e o apito final. Alguns ajustes de fuso horário foram feitos.

```r
dados$created_at <- as.POSIXct(dados$created_at, tz = "UTC")
dados$created_at <- dados$created_at - hours(3)

game_started_at <- as.POSIXct('2022-06-28 19:15:00', tz = 'UTC')
game_ended_at <- as.POSIXct('2022-06-28 21:10:00', tz = 'UTC')

dados <- dados %>%
  mutate(
    moment = case_when(
      created_at >= game_started_at & created_at <= game_ended_at ~ "game",
      created_at > game_ended_at ~ "post-game",
      TRUE ~ "pre-game"))
```

Com os tuítes selecionados, partimos para o gráfico. A parte mais trabalhosa é incluir as marcações nos momentos específicos do evento. A tarefa fica mais fácil no futebol, uma vez que a partida é cronometrada.

```r
gg <- dados %>%
  filter(moment == "game") %>%
  ggplot(aes(created_at)) +
  geom_freqpoly(binwidth = 60, size = 1.2, col = "#233B43") +
  xlab('Hora de Brasília') + ylab('') +
  scale_y_continuous(labels = comma) +
  labs(title = "Tweets #GALO vs. #EMELEC",
       subtitle = "Copa Libertadores - 28 de junho de 2022",
       caption = "Baseado no modelo criado por @vidigal_br",
       family = "sans") +
  coord_cartesian(clip = "off")
```

O resultado desse script é um gráfico de frequência de tuítes ao longo do jogo.

### Explorando o conteúdo

Com o banco de dados salvo, há inúmeras opções de análises mais qualitativas. Apenas para exploração, fiz uma nuvem de palavras para cada tempo de jogo.

No primeiro tempo, o foco foi o gol de Ademir. Em segundo plano, aparecem comentários sobre outros jogadores, a necessidade de "matar" o jogo e a previsão de empate. O engajamento foi maior e mais pessimista na parte final do jogo.

No segundo tempo, as palavras mais frequentes são ligadas ao pênalti e aos nomes de Allan, Hulk e Nathan. Também aparecem "turco" e "Palmeiras".

### O que engajou mais?

Também podemos coletar os tuítes com mais interações entre os 10 mil coletados.

1. Em primeiro lugar, o tuíte de @futebol_info noticiando o pênalti perdido: <https://twitter.com/futebol_info/status/1541934620098396161>
2. Em segundo lugar, o tuíte de @paposfut chamando a atenção para o prêmio de melhor da partida: <https://twitter.com/paposfut/status/1541915607419043841>
3. Em terceiro lugar, o tuíte de @Atletico anunciando o gol do Galo: <https://twitter.com/Atletico/status/1541912283445661697>

**Créditos**:

Parte do código foi retirada do site de Bruno Vidigal: Vidigal, B. (2020, Oct. 26). *Premier League: Twitter Data Visualization with R*.
