---
title: "Exercício de raspagem dos dados do Twitter"
image:
  caption: ' '
  placement: 0.3
date: "2022-06-30"
math: true
---
### Como raspar dados do Twitter?
**Atualização**: Com as mudanças na plataforma, o exercício e os scripts estão desatualizados.

### Contexto

Esse exercício tem o objetivo de apresentar as possibilidades de uso do **R** para o acompanhamento de reações nas redes sociais em eventos ao vivo. A escolha de uma partida do **Galo** foi aleatória e nada tem a ver com as preferências pessoais do autor. Você poderá aplicar o mesmo exercício para análises em campanhas políticas, por exemplo.

Utilizei como referência a análise que Bruno Vidigal fez para a Premier League. [Veja aqui](https://www.v-dem.net/data/reference-documents/).


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


Com os tuítes selecionados, partimos para o gráfico.

A parte mais trabalhosa é incluir as marcações nos momentos específicos do evento. A tarefa fica mais fácil no futebol, uma vez que a partida é cronometrada. 


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
  annotate("text", x = as.POSIXct("2022-06-28 19:31:00", tz = 'UTC'), y = 280, 
           label = "Gol do Galo \n (Ademir)", colour = '#111111', size = 6, family = "sans") +
  annotate("text", x = as.POSIXct("2022-06-28 20:30:00", tz = 'UTC'), y = 325, 
           label = "Gol do Emelec \n (Rodríguez)", colour = '#007cb0', size = 6, family = "sans") +
  annotate("text", x = as.POSIXct("2022-06-28 20:41:00", tz = 'UTC'), y = 470, 
           label = "Allan é expulso", colour = '#111111', size = 6, family = "sans") +
  annotate("text", x = as.POSIXct("2022-06-28 21:01:00", tz = 'UTC'), y = 420, 
           label = "Pênalti para o Galo", colour = '#111111', size = 6, family = "sans") +
  annotate("text", x = as.POSIXct("2022-06-28 20:10:30", tz = 'UTC'), y = 610, 
           label = "Intervalo", colour = '#233B43', size = 8, family = "sans") +
  annotate("text", x = as.POSIXct("2022-06-28 19:15:00", tz = 'UTC'), y = 610, 
           label = "1º Tempo", colour = '#233B43', size = 8, family = "sans") +
  annotate("text", x = as.POSIXct("2022-06-28 21:15:00", tz = 'UTC'), y = 610, 
           label = "2º Tempo", colour = '#233B43', size = 8, family = "sans") +
  geom_vline(xintercept = as.POSIXct("2022-06-28 20:03:00", tz = 'UTC'), linetype="dotted", 
             color = "#233B43", size=1.5) + 
  geom_vline(xintercept = as.POSIXct("2022-06-28 20:18:00", tz = 'UTC'), linetype="dotted", 
             color = "#233B43", size=1.5) +
  coord_cartesian(clip = "off")
```


O resultado desse script é o gráfico abaixo:

{{< figure src="Grafico.jpeg" caption="Gráfico de frequência de tuítes ao longo do jogo" numbered="false" >}}

### Explorando o conteúdo

Com o banco de dados salvo, há inúmeras opções de análises mais qualitativas. Apenas para exploração, fiz uma nuvem de palavras para cada tempo de jogo.

No 1º tempo, o foco foi o “gol” do “Ademir”. Em segundo plano, comentários sobre outros jogadores, a necessidade de “matar” o jogo, a previsão de um “empate”... O engajamento foi maior e mais pessimista na parte final do jogo.

{{< figure src="nuvem1.png" caption="Palavras mais frequentes no 1º tempo" numbered="false" >}}

No segundo tempo... o carinho da torcida! As palavras mais frequentes são ligadas ao pênalti, aos nomes de Allan, Hulk e Nathan. Aparecem também: “turco” e “Palmeiras”.

{{< figure src="nuvem2.png" caption="Palavras mais frequentes no 2º tempo" numbered="false" >}}



### O que engajou mais?

Também podemos coletar os tuítes com mais interações entre os 10 mil coletados.

1) Em primeiro lugar, o tuíte de @futebol_info noticiando o pênalti perdido.

{{< tweet 1541934620098396161 >}}

2) Em segundo lugar, o tuíte de @paposfut chamando a atenção para o prêmio de melhor da partida (ainda no primeiro tempo)

{{< tweet 1541915607419043841 >}}

3) Em terceiro lugar, O tuíte de @Atletico anunciando o gol do Galo.

{{< tweet 1541912283445661697 >}}


**Créditos**:
- Parte do código foi retirado do site de Bruno Vidigal
Vidigal (2020, Oct. 26). Bruno Vidigal: Premier League: Twitter Data Visualization with R. Retrieved from https://www.brunovidigal.com/posts/2021-01-10-premier-league-twitter-data-visualization-with-r/
