# Monitor de Voos — Fase 0: Validação de Dados

Nov 4, 2026 · @Zé Manel

## Alterações da versão final (4.1)

A versão 4.1 aplica as 9 correcções da quarta revisão, sem alterar a arquitectura, as fontes, os limiares nem o orçamento (353 pedidos). As notas A, B e C da v4 foram aceites pelo revisor; a nota B passa a aplicar-se também a C3. As versões anteriores estão no histórico.

| # | Correcção | Onde |
| --- | --- | --- |
| 1 | C2 exige companhia + número de voo + hora de partida; a equivalência entre `airline` (Travelpayouts) e `marketing_carrier_code` (Ignav) é testada com os primeiros dados recolhidos, sem pedidos extra; sem ela, a C2 é ambígua | O que se mede |
| 2 | A cotação de voo inclui `departure_at` e os aeroportos concretos | O que se mede, Modelo de dados |
| 3 | C3 e taxa do bebé exigem `verified` nas pesquisas I2 usadas | O que se mede, Critérios |
| 4 | Idade máxima útil cumulativa: uma faixa só conta se todas as mais recentes também passarem | Critérios |
| 5 | Cobertura classificada por resultado; ausência de dados nunca é ausência de voo | Configuração, Critérios |
| 6 | `fare_name` só se compara entre links do mesmo vendedor, com `specificity = exact_flight` | O que se mede |
| 7 | Critérios de escolha e substituição dos destinos; lista fechada antes de consultar a cobertura | Configuração, Amostragem |
| 8 | Idade negativa = hora incoerente, excluída da análise por idade | O que se mede |
| 9 | Nota sobre a reutilização de 30 minutos actualizada | Esta secção |

**Dois pormenores acrescentados ao aplicar as correcções**, para confirmação do revisor:

- **Números de voo e horas normalizados antes de comparar.** Os números de voo são comparados sem zeros à esquerda. As horas são convertidas para UTC, porque a Travelpayouts devolve `departure_at` com fuso e a Ignav devolve `departure_time_utc`.
- **A Ignav só devolve a companhia operadora por nome** (`operating_carrier_name`), não por código. Por isso a equivalência de companhias só se pode testar contra o código comercial.

**Reutilização de resultados da Ignav.** Segundo a FAQ oficial, consultada pelo revisor, os resultados da pesquisa exacta e do calendário podem ser reutilizados até 30 minutos e mantêm o `observed_at` original, que pode ser nulo; o `ignav_id` não é um identificador estável de voo. Não consegui abrir a FAQ a partir daqui. As regras temporais já tratavam estes casos e a identificação de voos usa sempre os dados dos segmentos.

**Alterações finais, após a revisão da 4.1** (sem pedidos, tabelas ou custos novos; orçamento de 353)

| # | Alteração | Localização exacta |
| --- | --- | --- |
| 1 | Teste de equivalência por correspondência de códigos, com pares candidatos definidos sem o código de companhia e pelo menos 10 pares por correspondência; inclui as salvaguardas aprovadas (sufixos do número de voo, fuso explícito, data local) | “O que se mede” › “Regras de correspondência em C2” (lista inteira) |
| 2 | O mapa de rotas só identifica candidatos; a elegibilidade exige horário ou calendário oficial nas duas janelas | “Configuração” › “Escolha dos destinos”, ponto 2 |
| 3 | Formulação prudente sobre a automação na Ryanair | “Configuração” › “Regras de fontes”, linha “Site da Ryanair” |
| 4 | Aeroportos devolvidos e hora de partida original e em UTC guardados | “Modelo de dados”, linhas `precos_tp` e `precos_ignav` |

## Objectivo

A Fase 0 decide, com dados medidos e limiares fixados antes do Dia 1, se a Travelpayouts serve para descobrir preços e se a Ignav serve para os confirmar. Nenhum limiar é alterado depois de vistos os dados; o que não puder ser medido é declarado não mensurável.

O teste responde a cinco perguntas:

1. Que percentagem das datas de cada rota tem preço em cada fonte?
2. Quanto difere o preço mínimo indicado pela Travelpayouts do preço mínimo real nessa data?
3. Como cresce essa diferença com a idade do preço?
4. A Ignav dá o preço correcto para 2 adultos + 1 bebé ao colo, face ao site da companhia?
5. Quantos pedidos à Ignav custa cada observação útil?

Fica fora do âmbito: o mapa, a interface, os alertas, as viagens de ida e volta e qualquer recolha automática nos sites das companhias.

## Configuração

O teste usa 18 rotas só de ida, a partir de FAO, LIS e SVQ, durante 7 dias, prolongáveis apenas nas células de decisão inconclusivas.

| Parâmetro | Valor |
| --- | --- |
| Origens | FAO, LIS, SVQ |
| Destinos | 6 por origem: 2 com Ryanair dominante, 2 com easyJet, 2 com outras companhias; lista fechada antes de qualquer consulta de cobertura (ver abaixo) |
| Passageiros | 1 adulto nas comparações com a Travelpayouts; 2 adultos + 1 bebé ao colo nas confirmações familiares |
| Tipo de viagem | Só ida |
| Escalas | `max_stops: 0` na Ignav; só linhas directas na Travelpayouts |
| Bagagem | Sem preferência (`carry_on_bags` e `checked_bags` omitidos), igual no calendário e na pesquisa exacta |
| Janela A | Partidas de D+30 a D+60 (31 dias, D = dia da recolha) |
| Janela B | Partidas de D+61 a D+91 (31 dias) |
| Mercado Ignav | PT; moeda confirmada na primeira resposta |
| Mercado Travelpayouts | Configurável por origem, escolhido no Dia 0 (ver abaixo) |
| Moeda Travelpayouts | `currency=eur` (por omissão seriam rublos) |

**Escolha do mercado da Travelpayouts no Dia 0**

A cache só contém pesquisas feitas em cada mercado, e T1 e T2 podem ter coberturas diferentes. Por isso mede-se a cobertura dos dois endpoints, por origem e por mercado, antes de escolher. A tabela é preenchida no Dia 0 e fica no relatório final.

| Origem | Mercado | Cobertura T1 | Cobertura T2 |
| --- | --- | --- | --- |
| FAO | `pt` / `es` / `us` | a medir | a medir |
| LIS | `pt` / `es` / `us` | a medir | a medir |
| SVQ | `pt` / `es` / `us` | a medir | a medir |

- **Cobertura T1** = datas com preço directo e `actual = true` ÷ datas pedidas, nas 6 rotas da origem e nas duas janelas.
- **Cobertura T2** = datas com voo directo devolvido ÷ datas pedidas, no mesmo conjunto.
- **Escolha:** o mercado com maior cobertura T1, por ser a da métrica principal. Em caso de empate em T1, o de maior cobertura T2. Se continuarem empatados, a ordem fixa `pt`, `es`, `us`.
- Uma origem cujo melhor mercado fique abaixo de 30 % em T1 é declarada não mensurável para a Travelpayouts. Uma cobertura T2 baixa fica registada à parte e explica as C2 em falta, que não se confundem com falta de correspondência de voos.

**Escolha dos destinos (antes de consultar qualquer API)**

1. Os destinos são indicados por código de aeroporto, nunca de cidade.
2. Para cada origem e grupo de companhia (Ryanair, easyJet, outras), o mapa oficial de rotas da companhia serve apenas para identificar destinos potencialmente elegíveis. Um destino só é elegível se o horário ou calendário oficial da companhia mostrar voos directos nas duas janelas. A verificação é manual.
3. Por origem e grupo regista-se uma lista ordenada: 2 destinos principais e 2 de reserva.
4. Um destino só é substituído pelo seguinte da reserva se a fonte oficial mostrar que a rota não opera nas janelas. Nunca por falta de cobertura numa API.
5. A lista final é registada com data e hora antes da primeira chamada T1 ou T2. Se um grupo não tiver destinos elegíveis numa origem, fica registado e a origem testa-se com menos rotas.

**Classificação de cada data pedida**

| Fonte | Classificações |
| --- | --- |
| T1 | Preço directo com `actual = true`; `actual = false`; `actual` desconhecido; sem resultado na cache; erro ou resposta inválida da API |
| T2 | Voo directo devolvido; sem resultado na cache; erro ou resposta inválida da API |
| I1, I2 | Preço devolvido; sem resultado; erro ou resposta inválida da API |

“Sem resultado” significa ausência de dados na fonte, nunca que não existe voo. Isto vale tanto para a cache da Travelpayouts como para a Ignav.

**Regras de fontes**

| Fonte | Utilização na Fase 0 |
| --- | --- |
| Travelpayouts | Recolha diária automática |
| Ignav | Calendário, pesquisas exactas e links de reserva automáticos |
| Site da Ryanair | Só confirmação manual nesta fase. Os termos consultados não têm uma proibição expressa equivalente à da easyJet para uso não comercial, mas isso não constitui autorização geral para qualquer método automatizado |
| Site da easyJet | Só confirmação manual. Sem qualquer automação, por proibição da política de utilização |
| Hermes | Não utilizado |
| Google Flights (scraping, SerpApi) | Não utilizado |

## Pedidos às APIs

São usados cinco pedidos, todos conferidos com a documentação REST oficial. Só os da Ignav contam para o orçamento, e um pedido de links que devolva 404 não é cobrado.

**T1. Travelpayouts — preço mínimo por data, com hora em que foi encontrado**

```
GET https://api.travelpayouts.com/v2/prices/month-matrix
  origin=FAO  destination=<IATA>  month=2026-12-01
  one_way=true  show_to_affiliates=false  market=<mercado da origem>
  currency=eur  limit=31
  Cabeçalho: X-Access-Token: <token>
```

Um pedido por rota e por cada mês abrangido pelas janelas A e B (3 ou 4 meses). Devolve por data: `depart_date`, `value`, `number_of_changes`, `found_at`, `actual`. Não identifica a companhia nem o voo. A cache guarda preços até 7 dias.

**T2. Travelpayouts — voo mais barato por data, com companhia e número de voo**

```
GET https://api.travelpayouts.com/aviasales/v3/prices_for_dates
  origin=FAO  destination=<IATA>  departure_at=2026-12
  one_way=true  direct=true  sorting=price  market=<mercado da origem>
  currency=eur  limit=31  page=1
  Cabeçalho: X-Access-Token: <token>
```

Um pedido por rota e por mês, como em T1. Com `one_way=true` devolve um único bilhete por data, encontrado nas últimas 48 horas, com `airline`, `flight_number` e `departure_at`. Não devolve `found_at`.

**Autenticação comum aos pedidos Ignav (I1, I2, I3)**

```
Cabeçalhos: X-Api-Key: <chave>
            Content-Type: application/json
```

Os três pedidos Ignav seguintes usam sempre estes dois cabeçalhos; não se repetem em cada exemplo.

**I1. Ignav — calendário familiar (1 pedido = até 31 datas)**

```json
POST https://ignav.com/api/fares/cheapest-dates
{
  "origin": "FAO", "destination": "<IATA>",
  "departure_date_range": { "start": "<D+30>", "end": "<D+60>" },
  "adults": 2, "infants_on_lap": 1,
  "max_stops": 0, "market": "PT"
}
```

Devolve, por data, o preço mais baixo para o grupo inteiro com taxas obrigatórias (ou `null`), com estado `unverified`, e um `observed_at` global. Não identifica o voo nem devolve `ignav_id`.

**I2. Ignav — pesquisa exacta**

```json
POST https://ignav.com/api/fares/one-way
{
  "origin": "FAO", "destination": "<IATA>",
  "departure_date": "<data>",
  "adults": 1, "max_stops": 0, "market": "PT"
}
```

Com `adults: 1` serve de referência para a Travelpayouts. Nas confirmações familiares usa `"adults": 2, "infants_on_lap": 1`. Devolve `observed_at` e todos os itinerários da data, cada um com `price.status`, `bags`, segmentos com `marketing_carrier_code` e `flight_number`, e `ignav_id`. Não devolve links.

**I3. Ignav — links de reserva**

```json
POST https://ignav.com/api/fares/booking-links
{ "ignav_id": "<ignav_id de I2, mesma conta>" }
```

Só para a verificação manual, e para os dois itinerários de cada confirmação (sem bebé e com bebé), para comparar o fare\_name. Devolve `booking_options[].links[]` com vendedor, preço e `url`. Um 404 `itinerary_not_found` não é cobrado.

## O que se mede

Há dois tipos de medida que nunca se misturam: o **desvio entre mínimos por data** (C1, C4), em que os dois preços podem ser de voos diferentes, e o **erro de preço** (C2, C3), que só existe quando o voo coincide sem ambiguidade. As métricas decisórias usam apenas referências `verified` e comparações com tempo conhecido e válido.

**Quatro comparações**

| Comparação | Medida | Preço indicado | Preço de referência | Correspondência |
| --- | --- | --- | --- | --- |
| C1 — por data (principal para a descoberta) | Desvio entre mínimos | T1: `value` da data, com `number_of_changes = 0` e `actual = true` | I2 com 1 adulto: directo mais barato da data, classificado pelo seu `price.status` | Rota + data. Pode ser de voos diferentes |
| C2 — por voo | Erro de preço | T2: preço do voo da data | I2 com 1 adulto: itinerário com o mesmo `marketing_carrier_code` + `flight_number`, classificado pelo seu `price.status` | Aeroportos + data + companhia + n.º de voo + hora de partida em UTC (regras abaixo) |
| C3 — familiar | Erro de preço | I2 com 2 adultos + bebé, só com estado verified para decidir | Preço base no site da companhia, mesmo voo, sem extras adquiridos | Companhia + n.º de voo + hora + condições comparáveis |
| C4 — calendário (descritiva) | Desvio entre mínimos | I1: preço da data | I2 familiar: o mais barato da data | Rota + data |

Em C2, se o voo da Travelpayouts não aparecer na Ignav, a observação é **não comparável**: não conta como erro nem como divergência.

**Regras de correspondência em C2**

- **Normalização:** no número de voo, só a parte numérica perde os zeros à esquerda; sufixos alfabéticos mantêm-se. As horas só são convertidas para UTC se trouxerem fuso explícito (`departure_at` da Travelpayouts; `departure_time_utc` da Ignav); uma hora sem fuso nunca é interpretada com o fuso do servidor e torna o par não comparável. A data de partida compara-se na data local do aeroporto de origem.
- **Par candidato:** mesmos aeroportos concretos, mesma data local, mesmo número de voo normalizado e mesmo instante de partida em UTC. O código de companhia ainda não entra nesta fase.
- **Teste de equivalência, por correspondência de códigos** (com os dados dos Dias 1 e 2, sem pedidos extra): para cada par de códigos `airline` (Travelpayouts) e `marketing_carrier_code` (Ignav) observado nos pares candidatos, exigem-se pelo menos 10 pares. Com 10 ou mais e todos coincidentes, essa correspondência é aceite. Com alguma divergência, fica **ambígua**. Com menos de 10 pares, fica **não mensurável**. O resultado de uma correspondência nunca se generaliza a outras companhias ou correspondências não testadas. O mínimo de 10 é um critério operacional deste teste, não uma prova de equivalência universal.
- **Correspondência inequívoca (C2 válida):** par candidato cuja correspondência de códigos foi aceite. As C2 de correspondências ambíguas ou não mensuráveis ficam fora das métricas e são contadas à parte.
- A Ignav só identifica a companhia operadora por nome (`operating_carrier_name`). Esse nome é registado, mas não entra na correspondência.

**Estado da referência Ignav (C1 e C2)**

| Estado | Tratamento |
| --- | --- |
| `verified` | Entra nas métricas decisórias |
| `unverified` | Medida e apresentada à parte; nunca entra nas métricas decisórias |
| Sem referência utilizável (nenhum directo, ou voo não encontrado em C2) | Contada à parte |

Se as referências `verified` não chegarem ao mínimo, a célula fica inconclusiva; não se completa a amostra com `unverified`.

**Campo `actual` de T1**

| Valor | Tratamento |
| --- | --- |
| `true` | Entra na análise principal e na cobertura. Não garante que o preço esteja disponível ao reservar: é só o indicador da fonte |
| `false` | Oferta não actual: registada, fora da cobertura e de C1, contada à parte |
| Nulo ou ausente | Estado desconhecido: registado, fora da cobertura e de C1, contado à parte |

**Classificações de um preço que não se confirma**

| Situação | Classificação | Onde |
| --- | --- | --- |
| A referência da data excede o mínimo indicado em mais de 10 % | Desvio superior a 10 % (descritivo) | C1 |
| O mesmo voo está na Ignav pelo menos 1 € mais caro do que o indicado | Divergência desfavorável da Travelpayouts: indicador, não prova de indisponibilidade | C2 |
| O site da companhia não tem o voo ou o preço | Indisponível confirmado | Só C3 |

**Relógios e regras temporais**

| Campo | Significado | Fonte |
| --- | --- | --- |
| `found_at` | Quando a Travelpayouts encontrou o preço | T1 |
| `collected_at` | Quando a nossa recolha recebeu a resposta | Nosso registo |
| `observed_at` | Quando a Ignav obteve o resultado; pode ser anterior ao pedido se a resposta for reutilizada, e pode ser nulo | I1, I2 |
| `confirmed_at` | Quando foi feita a verificação manual | Nosso registo |

- **Idade do preço** = `collected_at` (T1) − `found_at`.
- **C1 e C2:** válidas só se `0 ≤ observed_at (I2) − collected_at (T1 ou T2) ≤ 2 h`. Um valor negativo é excluído e contado como “resposta anterior à recolha”; acima de 2 h, como “atraso excessivo”.
- **C3:** válida só se `0 ≤ confirmed_at − observed_at (I2 com bebé) ≤ 30 min`.
- **C4:** válida só se os `observed_at` de I1 e de I2 familiar distarem no máximo 5 minutos, usando os valores devolvidos e não a hora de envio.
- **`observed_at` nulo:** a comparação é guardada com tempo desconhecido e fica fora de todas as métricas que dependem destes limites. Nunca se substitui por `collected_at`.

**Horas incoerentes.** Todas as horas são convertidas para UTC antes de qualquer cálculo. Uma idade negativa (`found_at` posterior a `collected_at`) é classificada como hora incoerente, excluída da análise por idade e contada à parte. Nunca entra na faixa 0–24 h.

**Unidades de contagem**

| Análise | Unidade | Contagens apresentadas |
| --- | --- | --- |
| C1 geral | Rota + data de partida; conta a primeira comparação válida | `n_unico` (decide) e `n_total` |
| Idade da cache | Cotação = rota + data + `value` + `found_at` de T1. Uma cotação conta no máximo uma vez em cada faixa de idade | Cotações distintas por faixa (decide) e medições repetidas da mesma cotação (descritivo) |
| C2 | Cotação de voo = aeroportos + data + companhia + n.º de voo + hora de partida em UTC + preço de T2 | Cotações de voo distintas (decide) e medições repetidas |
| C3 | Voo confirmado; cada voo só é confirmado uma vez | n |

As medições repetidas da mesma cotação nunca são apresentadas como observações independentes.

**Data de preço mediano**

Para cada rota e janela, num dado dia de recolha, ordenam-se as datas com preço directo e `actual = true` em T1 pelo `value`. A “data de preço mais baixo” é a primeira; a “data de preço mediano” é a da posição ⌈n/2⌉. É uma mediana entre datas de partida, não entre voos da mesma data.

**Bagagem em C3**

Nas pesquisas Ignav, omitir `carry_on_bags` e `checked_bags` significa “sem preferência”, não zero bagagem. C3 compara o preço base sem extras adquiridos e regista a franquia devolvida pela Ignav (`bags`) e a franquia mostrada no site. Se as franquias forem diferentes de forma que afecte o preço, a C3 é não comparável.

**Taxa do bebé**

Em cada C3 fazem-se duas pesquisas I2 seguidas (menos de 5 minutos entre os `observed_at`), sem bebé e com bebé, e um I3 para cada itinerário. A diferença só entra nas métricas decisórias se as duas pesquisas I2 tiverem `price.status = verified` e coincidirem companhia, número de voo, hora, `cabin_class` e `bags`. Com alguma pesquisa `unverified`, a observação é guardada e apresentada como não verificada, fora dos denominadores.

O `fare_name` só é comparado entre links do **mesmo vendedor**, de preferência a própria companhia, ambos com `specificity = exact_flight`. Sem um par assim, o `fare_name` fica não comparável e a taxa é marcada “sem comparação de tarifa”, apresentada à parte. Nunca se compara o nome de tarifa de uma companhia com o de uma agência. Qualquer divergência nos campos comparados torna o resultado **inconclusivo**.

## Amostragem e calendário

Por dia, fazem-se 18 comparações C1 (uma por rota) e 4 confirmações familiares C3. Cada análise conta na sua própria unidade (secção “O que se mede”); as medições repetidas nunca aumentam o n que decide.

**Selecção diária das datas para C1**

1. Cada rota recebe uma comparação por dia, alternando a janela (A nos dias ímpares, B nos pares).
2. Alterna-se também a posição: data de preço mais baixo num dia, data de preço mediano no seguinte.
3. A partir do Dia 4, se uma faixa de idade tiver menos de 10 cotações distintas, escolhe-se a data cuja cotação caia nessa faixa, em vez da posição prevista. Pode ser uma cotação já vista noutra faixa; fica marcada como repetida. Se não houver nenhuma, regista-se “sem observações disponíveis”.
4. A mesma rota + data de partida só é comparada uma vez por dia.

**Confirmações familiares C3**

Quatro por dia: 2 Ryanair, 1 easyJet e 1 de outra companhia, rodando as três origens; em 7 dias, 14 Ryanair, 7 easyJet e 7 outras. Cada voo só é confirmado uma vez. Os pedidos são feitos quando se abre a página de verificação, por esta ordem:

1. I1 da janela da data e, logo a seguir, I2 com 2 adultos + bebé (C4: no máximo 5 minutos entre os `observed_at`).
2. I2 com 2 adultos, sem bebé (taxa do bebé: no máximo 5 minutos face ao I2 com bebé).
3. I3 para os dois itinerários.
4. Preço base no site da companhia, até 30 minutos depois do `observed_at` do I2 com bebé.

**Calendário**

1. **Dia 0:** lista dos 18 destinos fechada e registada, antes de qualquer chamada T1 ou T2; registo nas duas plataformas; uma chamada de cada pedido para confirmar campos, moeda e preenchimento de `observed_at` e `actual`; cobertura de T1 e T2 por origem nos mercados `pt`, `es` e `us`, e escolha do mercado de cada origem; teste de I3 em duas rotas.
2. **Dias 1 a 7, de manhã (automático):** T1 e T2 para as 18 rotas e todos os meses das janelas; logo a seguir, 18 pesquisas I2 com 1 adulto (C1 e, quando o voo coincide, C2). Nos Dias 1 e 7, também I1 para as 18 rotas e as duas janelas, para medir a cobertura do calendário.
3. **Dias 1 a 7, verificação manual (cerca de 10 minutos):** as 4 confirmações C3, pela ordem acima.
4. **Dia 8:** relatório com todas as contagens e a decisão de cada célula: aprovada, reprovada, inconclusiva ou não mensurável. Só as inconclusivas se prolongam, por mais 7 dias.

## Modelo de dados

As mesmas sete tabelas da v3, sem tabelas novas. Cada comparação aponta para os registos de preço e os pedidos de que veio, para qualquer conclusão poder ser reconstruída.

| Tabela | Uma linha por | Campos principais |
| --- | --- | --- |
| `rotas` | Rota configurada | origem, destino, grupo de companhia esperado, mercado Travelpayouts da origem, activa |
| `pedidos_api` | Pedido feito | fonte, endpoint (T1, T2, I1, I2, I3), parâmetros, estado HTTP, cobrado (sim/não), `collected_at` |
| `precos_tp` | Data devolvida por T1 ou T2 | id do pedido, rota, mercado, data de partida, preço, moeda, escalas, `found_at` e `actual` (só T1), companhia, n.º de voo, aeroportos devolvidos (origin\_airport, destination\_airport), departure\_at original com fuso e o seu instante UTC (só T2), id da cotação |
| `precos_ignav` | Data de I1, itinerário de I2 ou link de I3 | id do pedido, rota, data, passageiros, preço, moeda, `price.status`, companhia comercializadora (marketing\_carrier\_code), operadora (operating\_carrier\_name), n.º de voo, aeroportos de partida e chegada dos segmentos, departure\_time\_local, departure\_timezone e departure\_time\_utc, `cabin_class`, `bags`, `fare_name` e vendedor (só I3), `ignav_id`, `observed_at` |
| `comparacoes` | Comparação C1 a C4 | nível, medida, id do preço indicado, id da referência, ids dos pedidos de ambos, id da confirmação manual (C3), estado da referência (`verified`, `unverified`, sem referência), estado temporal (válido, anterior à recolha, atraso excessivo, desconhecido), estado da comparação (válida, não comparável, condições diferentes), classificação, idade, faixa, posição, única (sim/não), id da cotação, repetição (sim/não) |
| `confirmacoes_site` | Verificação manual | companhia, n.º de voo, tarifa mostrada, franquia de bagagem mostrada, preço base, taxa do bebé mostrada, disponível (sim/não), `confirmed_at` |
| `taxas_bebe` | Medição da taxa | ids dos dois itinerários I2 e dos dois I3, id da confirmação manual, companhia, mercado, rota, data, valor, moeda, campos comparados, `fare_name` comparado (sim/não), estado (coincide, não coincide, inconclusiva) |

**Valores calculados (nunca introduzidos à mão)**

- Idade do preço = `collected_at` − `found_at`, em horas; faixas 0–24 h, 24–48 h, 48–72 h, mais de 72 h.
- Id da cotação = rota + data + `value` + `found_at` (T1), ou aeroportos + data + companhia + n.º de voo + hora de partida em UTC + preço (T2).
- Desvio ou erro relativo = (referência − indicado) ÷ referência; positivo quando a referência é mais cara.
- Desvio ou erro absoluto = |desvio ou erro relativo|.
- Estado temporal, calculado pelas regras da secção “O que se mede”.

## Critérios de aprovação

Os valores dos limiares são os da versão 1, sem alterações. As métricas que decidem usam só comparações com referência `verified`, estado temporal válido e a unidade de contagem própria de cada análise. Os resultados mostram sempre todas as contagens, incluindo as excluídas, sem alegações de significância estatística.

**Métricas e denominadores**

| Métrica | Numerador | Denominador |
| --- | --- | --- |
| Cobertura T1 | Datas com preço directo e `actual = true` em T1 | Datas pedidas (rota × janela × dia; 31 por janela) |
| Desvio típico (C1) | Mediana do desvio absoluto | C1 decisórias e únicas da célula |
| Desvio extremo (C1) | Percentil 90 do desvio absoluto (só com n ≥ 10) | C1 decisórias e únicas da célula |
| Desvio superior a 10 % (C1, descritivo) | C1 com referência acima do indicado em mais de 10 % | C1 decisórias e únicas |
| Divergência desfavorável (C2) | C2 em que o mesmo voo está na Ignav pelo menos 1 € mais caro | Cotações de voo distintas com C2 decisória |
| Lacuna de companhia (C2, descritivo) | C2 com outra companhia mais barata na mesma data | Cotações de voo distintas com C2 decisória |
| Excluídas (C1 e C2, descritivo) | Por motivo: `unverified`, sem referência, não comparável, anterior à recolha, atraso excessivo, tempo desconhecido, `actual` não verdadeiro | Todas as tentadas |
| Acerto da Ignav (C3) | C3 com erro absoluto ≤ 5 % face ao site | C3 válidas, com I2 familiar verified, voo disponível e condições comparáveis |
| Bebé correcto (C3) | C3 em que a diferença Ignav (com bebé − sem bebé) coincide com a taxa do site ± 2 € | C3 com taxa conclusiva e as duas pesquisas I2 verified |
| Desvio do calendário (C4, descritivo) | Mediana e percentil 90 do desvio absoluto entre I1 e I2 familiar | C4 válidas (≤ 5 minutos) |
| Datas sem referência (C4, descritivo) | C4 em que I1 tem preço mas I2 familiar não tem directo, ou vice-versa | Todas as C4 tentadas |

**Células de decisão e limiares**

| Fonte e papel | Célula | n mínimo (meta) | Aprovada se |
| --- | --- | --- | --- |
| Travelpayouts, descoberta | Cada origem (3 células) | 10 C1 únicas (30) | Cobertura ≥ 60 %; desvio típico ≤ 15 %; desvio extremo ≤ 35 % |
| Travelpayouts, actualidade dos preços | Todas as origens juntas (1 célula) | 10 cotações de voo (30) | Divergência desfavorável ≤ 30 % |
| Travelpayouts, idade máxima útil | Cada faixa de idade, origens juntas (4 células) | 10 cotações distintas (30) | Faixa mais antiga até à qual todas as faixas mais recentes também passam (desvio típico ≤ 15 % e desvio extremo ≤ 35 %). Uma faixa reprovada ou inconclusiva interrompe a contagem: as faixas mais antigas não recuperam a aprovação |
| Ignav, confirmação: preço | Todas as C3 juntas | 20 C3 válidas (28) | Acerto ≥ 80 %; itinerários `verified` com erro típico ≤ 2 % (só avaliado com pelo menos 10) |
| Ignav, confirmação: bebé | Todas as C3 juntas | 10 C3 com taxa conclusiva e I2 verified | Bebé correcto ≥ 90 % |

Abaixo do mínimo não se apresenta percentagem como resultado de aprovação: a célula é inconclusiva.

**Taxa do bebé por companhia**

Com 2 ou mais observações conclusivas que coincidam com o site, o resultado é “verificação inicial positiva”. Mostra que o método funciona para essa companhia nas rotas observadas; não valida a taxa em geral nem para outras rotas, datas ou tarifas.

**Regras de decisão**

- Célula abaixo do mínimo: **inconclusiva**; prolonga-se só essa célula. Nunca se completa a amostra com comparações excluídas.
- Métrica sem forma de ser obtida com estas fontes: **não mensurável**, nunca tratada como aprovada.
- Uma fonte só é aprovada para um papel se todas as suas células desse papel forem aprovadas. Se alguma for não mensurável, a decisão passa para o utilizador, com o parecer dos dois revisores.
- C4, os cruzamentos origem × faixa de idade e os resultados por companhia em C3 são descritivos: apresentam-se com o seu n, mas não aprovam nem reprovam nenhuma fonte nesta fase.

## Orçamento

O teste de 7 dias gasta cerca de 353 dos 1.000 pedidos gratuitos da Ignav; com o prolongamento máximo de mais 7 dias, chega a cerca de 619. Custo: zero.

| Operação Ignav | Para quê | Cálculo | Pedidos |
| --- | --- | --- | --- |
| Dia 0 | Confirmar campos, mercado de SVQ, testar I3 | estimativa com margem | 15 |
| I2 com 1 adulto | C1 e C2 | 18 rotas × 7 dias | 126 |
| I2 sem bebé | Taxa do bebé | 4 voos × 7 dias | 28 |
| I2 com bebé | C3 e taxa do bebé | 4 voos × 7 dias | 28 |
| I3 para os dois itinerários | Link para o site e `fare_name` | 2 × 4 voos × 7 dias | 56 |
| I1 da janela de cada C3 | C4 | 4 voos × 7 dias | 28 |
| I1 nos Dias 1 e 7 | Cobertura do calendário | 18 rotas × 2 janelas × 2 dias | 72 |
| **Total do teste** |  |  | **353** |
| Prolongamento máximo | C1, C2, C3 e C4 durante mais 7 dias | 126 + 28 + 28 + 56 + 28 | 266 |

**Porque se repete I1 em cada C3.** Em produção, o calendário da Ignav (I1) é o que escolhe as datas a confirmar, por isso é preciso saber se o seu preço coincide com o da pesquisa exacta familiar. Essa comparação (C4) só é justa se as duas pesquisas forem feitas à mesma hora; as recolhas I1 dos Dias 1 e 7 são feitas de manhã e não servem para isso. Alternativas para decisão:

| Opção | Pedidos I1 em C3 | Consequência |
| --- | --- | --- |
| A — manter (recomendada) | 28 | C4 com cerca de 28 observações |
| B — só nos Dias 1, 3, 5 e 7 | 16 | C4 com cerca de 16 observações |
| C — retirar | 0 | C4 não mensurável nesta fase |

O prolongamento só se aplica às células inconclusivas, por isso o consumo real será menor. Os pedidos I3 que devolvam 404 não são cobrados.

**Travelpayouts:** 18 rotas × 3 a 4 meses × 2 endpoints = 108 a 144 pedidos por dia, sem custo, distribuídos ao longo da manhã para respeitar os limites da API. No Dia 0 multiplicam-se por 3 durante a escolha do mercado.

**Produção (a recalcular com o consumo real):** cerca de 10 a 15 pedidos Ignav por dia, ou 300 a 450 por mês, incluindo os pedidos de links. Esgotada a oferta inicial, são 0,60 a 0,90 USD por mês, dentro do limite de 1–2 €.

## Pressupostos a confirmar no Dia 0

Oito pressupostos só se confirmam com as primeiras chamadas reais; cada um tem uma consequência definida se falhar.

| # | Pressuposto | Se falhar |
| --- | --- | --- |
| 1 | A Ignav com `market: "PT"` devolve preços em EUR | Converter com a taxa do dia e registar a conversão |
| 2 | T1 devolve `found_at` em todas as datas, com `one_way=true` e `show_to_affiliates=false` | Datas sem `found_at` ficam fora de C1 e contam como “idade desconhecida” |
| 3 | O preço da Travelpayouts é para 1 adulto (não está documentado) | Se as primeiras 18 comparações C1 mostrarem uma razão sistemática próxima de 2, o teste pára para rever |
| 4 | Pelo menos um dos mercados `pt`, `es` ou `us` tem cobertura ≥ 30 % em cada origem, com mercado escolhido por origem | A origem sem cobertura fica “não mensurável” para a Travelpayouts |
| 5 | A Ignav devolve voos Ryanair e easyJet nestas origens | A confirmação dessa companhia passa a ser só manual, e regista-se como lacuna |
| 6 | I3 devolve links directos para as companhias usadas | Abrir a pesquisa no site da companhia à mão |
| 7 | `observed_at` vem preenchido | As comparações ficam com tempo desconhecido e fora das métricas temporais. Se forem a maioria, C1, C2 e C3 ficam inconclusivas e a decisão passa para o utilizador e os dois revisores |
| 8 | Os limites de pedidos da Travelpayouts comportam 144 pedidos por manhã | Espaçar a recolha ao longo do dia |

**Próximos passos**

- [ ] Aplicar a regra de colaboração: cada assistente propõe e justifica a ferramenta a usar, pede ao outro que use as suas quando forem melhores, questiona as escolhas do outro, e os factos decisivos são verificados pelos dois de forma independente (Claude e ChatGPT)
- [ ] Revisão final do ChatGPT no pull request do GitHub, confirmando que o documento publicado corresponde a esta versão (ChatGPT e utilizador)
- [ ] Aprovação final dos limiares, antes do Dia 1 (utilizador)
- [ ] Criar conta de afiliado na Travelpayouts e obter o token (utilizador)
- [ ] Criar conta na Ignav e obter a chave da API (utilizador)
- [ ] Passar o documento aprovado para o repositório GitHub, num pull request revisto pelo ChatGPT; só depois criar as tabelas e as tarefas agendadas no Supabase (Claude)
- [ ] Executar o Dia 0 e rever estes 8 pressupostos (Claude)

**Fontes** (consultadas a 4 de Novembro de 2026)

- [Travelpayouts — Aviasales Data API (endpoints, campos, mercados, cache)](https://support.travelpayouts.com/hc/en-us/articles/203956163)
- [Ignav — Cheapest Dates](https://ignav.com/docs/cheapest-dates)
- [Ignav — One-Way Fares](https://ignav.com/docs/one-way)
- [Ignav — Booking Links](https://ignav.com/docs/booking-links)
- [Ignav — Preços](https://ignav.com/pricing)
- [Ryanair — Termos de utilização (PT)](https://www.ryanair.com/pt/pt/lp/informacao-legal/termos-de-utilizacao)
- [easyJet — Política de utilização aceitável (PT)](https://www.easyjet.com/pt/policy/acceptable-use)
