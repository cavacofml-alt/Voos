# Great Escape — Referência para a Fase 1

4 de Novembro de 2026 · @Zé Manel

## Resumo

As fontes públicas descrevem o Great Escape como um mapa de preços mínimos por destino, com filtros de orçamento, voos directos, vistos, clima e popularidade, e com queixas recorrentes sobre preços que não correspondem à realidade. Quase toda a informação é de 2019 a 2021; o estado actual do site só se confirma usando-o.

Este documento é uma referência para desenhar a Fase 1 (interface). Não altera a Fase 0, cuja especificação está concluída, pendente da revisão final no PR #1.

**Aviso importante: os termos de utilização do Great Escape**

Os termos do site (em vigor desde 1 de Setembro de 2020) proíbem:

- usar robôs, *spiders* ou qualquer meio automático para aceder ao serviço, para qualquer fim, incluindo monitorizar ou copiar conteúdo;
- usar processos manuais para monitorizar ou copiar conteúdo sem consentimento escrito prévio;
- reutilizar o conteúdo para fins comerciais ou ganho pessoal sem autorização.

Consequências para o nosso trabalho:

1. **O navegador do Claude (ou outro agente automático) não deve navegar no greatescape.co.** Um agente que controla o navegador é um meio automático de acesso.
2. **A observação do tráfego de rede do site fica excluída**, porque encaixa na proibição de monitorizar o serviço. A arquitectura do nosso projecto já está decidida e não depende disso.
3. **A revisão no computador passa a ser feita pelo utilizador, à mão**, a observar funcionalidades e a descrevê-las por palavras próprias, sem copiar preços nem conteúdo. Em alternativa, pede-se consentimento escrito à empresa (ver o documento de instruções).

A recolha deste documento usou fontes de terceiros (imprensa, lojas de aplicações, Product Hunt, LinkedIn) e duas páginas públicas do próprio site, “Sobre” e “Termos”, lidas uma vez cada.

**Proveniência da leitura das páginas do Great Escape**

- A consulta foi feita pelo Claude da conversa web, a 4 de Novembro de 2026, com a ferramenta de leitura web, uma única vez para cada uma das duas páginas de `webdev.greatescape.co`.
- Foi anterior ao conhecimento dos termos. A proibição foi descoberta durante essa leitura.
- Não houve nenhum acesso posterior. Não houve consentimento escrito, navegação, uso de APIs internas nem recolha de preços.
- A informação vinda dessas duas páginas (por exemplo a cobertura declarada em F11, a origem da empresa e a tecnologia do site) provém de uma consulta pontual, sem novas verificações automáticas. A regra geral está em `AGENTS.md` e em D-010.

## Funcionalidades

As fontes concordam no essencial: escolhe-se a origem e as datas, deixa-se o destino em branco e o mapa mostra o preço mínimo para cada destino, com filtros. A coluna “Confiança” indica se várias fontes independentes o confirmam e se a informação é recente.

| # | Funcionalidade | O que dizem as fontes | Ano da fonte | Confiança |
| --- | --- | --- | --- | --- |
| F1 | Mapa colorido por preço | Rotas a verde (mais baratas) até vermelho (mais caras), em comparação com as outras rotas das mesmas datas ([FlyerTalk](https://www.flyertalk.com/?p=59962), [My Modern Met](https://mymodernmet.com/cheap-flights-escape-mit/)) | 2019 | Alta, mas antiga |
| F2 | Pesquisa sem destino | Origem e datas; destino em branco para ver todos ([FlyerTalk](https://www.flyertalk.com/?p=59962)) | 2019 | Alta, mas antiga |
| F3 | Filtros | Orçamento, só directos, vistos, popularidade e clima ([The Points Guy](https://thepointsguy.com/news/new-escape-travel-tool)); vistos por nacionalidade ([Lonely Planet](https://www.lonelyplanet.com/news/escape-flight-search-engine-map)); inclui visto à chegada e e-visa ([Product Hunt](https://www.producthunt.com/posts/escape-3/reviews)); região ([Product Hunt](https://www.producthunt.com/products/escape-travel-inspiration-by-price/reviews)) | 2019–2021 | Alta |
| F4 | Agregação geográfica | Preço mínimo por continente ([The Points Guy](https://thepointsguy.com/news/new-escape-travel-tool)); destino mais barato por país e depois por aeroporto ([Matador](https://matadornetwork.com/read/cheap-flight-search-tool/amp/)) | 2019 | Média |
| F5 | Só ida | Acrescentado em 2019 por ser o pedido mais frequente dos utilizadores ([Lonely Planet](https://www.lonelyplanet.com/news/escape-flight-search-engine-map), [Reddit](https://reddit.birdcat.cafe/r/dataisbeautiful/comments/b5qpfw/update_we_created_a_tool_to_visualise_the)) | 2019 | Alta, mas antiga |
| F6 | Informação do destino | Pontos de interesse, imagens e “o que fazer” ([FlyerTalk](https://www.flyertalk.com/?p=59962)); clima, segurança e vistos na versão 2.0 ([Product Hunt](https://www.producthunt.com/products/escape-travel-inspiration-by-price)); restrições Covid na app Android ([Google Play](https://play.google.com/store/apps/details?id=com.greatescape&hl=en)) | 2019–2021 | Média |
| F7 | Exclusões e faixas de preço | Faixas de preço, só directos, bloqueio de companhias e destinos ([CardRates](https://www.cardrates.com/news/escape-offers-flight-bargains-and-flexible-scheduling/)) | 2019 | Baixa (fonte única) |
| F8 | Classe executiva | Anunciada como “em desenvolvimento” ([CardRates](https://www.cardrates.com/news/escape-offers-flight-bargains-and-flexible-scheduling/)) | 2019 | Desconhecida |
| F9 | Datas flexíveis | Prometidas para “dentro de dois meses” ([FlyerTalk](https://www.flyertalk.com/?p=59962)); um utilizador diz que datas específicas exigem a versão paga ([App Store](https://apps.apple.com/us/app/id1511294506)) | 2019 / sem data | Contraditória |
| F10 | Reserva | Não vende bilhetes: encaminha para a Kiwi ([FlyerTalk](https://www.flyertalk.com/?p=59962)) ou outras plataformas como a Skyscanner ([Mental Floss](https://www.mentalfloss.com/transportation/planes-airports/cheapest-flights-online-tool)) | 2019 | Alta, mas antiga |
| F11 | Cobertura declarada | Mais de 50 pontos de dados por destino, mais de 300 companhias e 150 agentes ([página “Sobre”](https://webdev.greatescape.co/about)) | Sem data (© 2023) | Declaração do próprio |
| F12 | Aplicações móveis | A app iOS tem menos funções do que o site ([App Store](https://apps.apple.com/us/app/id1511294506)); última versão Android conhecida 1.0.5, datada de 2023 ou de Outubro de 2024 consoante o campo da mesma página ([APKCombo](https://apkcombo.com/escape-explore-flight-deals/com.greatescape/)) | 2024 | Média |
| F13 | Passageiros e bebés | Nenhuma fonte o refere | — | Lacuna |

## Fiabilidade e queixas

A queixa mais repetida é a mesma que a Fase 0 vai medir: o preço mostrado não corresponde ao que se encontra depois. Aparece desde o lançamento, em 2019, até às avaliações das lojas de aplicações.

| # | Problema relatado | Fonte | Ano |
| --- | --- | --- | --- |
| Q1 | Utilizadores encontraram tarifas mais baratas no Google Flights e na Kayak do que as mostradas como “mais baratas”; numa rota, o mínimo mostrado era várias vezes superior ao de outros sites | [Reddit, r/dataisbeautiful](https://axebps-redlib.hf.space/r/dataisbeautiful/comments/b5qpfw/update_we_created_a_tool_to_visualise_the/ejf697h) | 2019 |
| Q2 | O preço mostrado nem se aproxima do preço real | [App Store](https://apps.apple.com/us/app/id1511294506) | Sem data |
| Q3 | A app iOS encaminha para um link da Kayak que diz que não há voos; mostra poucas cidades e não deixa mudar preferências | [App Store](https://apps.apple.com/us/app/id1511294506) | Sem data |
| Q4 | A app Android bloqueia ou fecha com frequência; a experiência é complicada, também no site | [Google Play](https://play.google.com/store/apps/details?id=com.greatescape&hl=en) (3,2 em 126 avaliações) | Sem data |
| Q5 | O mapa ao vivo, antes gratuito, passou a ser pago | [Google Play](https://play.google.com/store/apps/details?id=com.greatescape&hl=en) | Sem data |

As fontes não dizem se o site mostra a idade dos preços ou avisa que são estimativas. Nos termos, a empresa não garante a exactidão do serviço, mas isso é uma cláusula genérica, não um aviso ao utilizador junto dos preços.

## Negócio, empresa e tecnologia

O Great Escape é uma empresa pequena de Singapura, sem financiamento externo, que vive de subscrições e de uma versão para companhias aéreas. O site usa Next.js e mapas da Mapbox.

| Tema | O que se sabe | Fonte |
| --- | --- | --- |
| Origem | Criado em 2018 no Singapore-MIT Alliance for Research and Technology; empresa GREAT ESCAPE PTE LTD, de Singapura | [LinkedIn](https://www.linkedin.com/company/greatescapeco), [página “Sobre”](https://webdev.greatescape.co/about) |
| Dimensão | Fundada em 2019, sem financiamento, 7 funcionários em 30 de Junho de 2026 | [Tracxn](https://tracxn.com/d/companies/great-escape/__6TPgSSFxiEhWxlg1Zdjq4NFJ24GVnguSeQnwwNLUdH0) |
| Alcance declarado | Mais de 1,5 milhões de viajantes “no último ano” (sem data) | [LinkedIn](https://www.linkedin.com/company/greatescapeco) |
| Receitas | Subscrição com renovação automática, sem período grátis, reembolso em 7 dias; versão “white label” para companhias aéreas | [Termos](https://webdev.greatescape.co/terms), [LinkedIn](https://www.linkedin.com/company/greatescapeco) |
| Dados de voos | Kiwi e Skyscanner, segundo a imprensa de 2019 | [TravelPulse](https://www.travelpulse.com/news/airlines/new-tool-finds-you-the-cheapest-flights-to-every-city-in-the-world.html) |
| Tecnologia do site | Next.js (página montada no navegador); mapa da Mapbox, agradecida nos créditos; início de sessão com Apple | Página principal e [página “Sobre”](https://webdev.greatescape.co/about) |

A Mapbox é gratuita só até certo volume de utilização. Para uso pessoal, o Leaflet com OpenStreetMap, que já tínhamos previsto, chega e não tem custo.

## Lições e lacunas

A lição principal confirma a prioridade do projecto: um mapa bonito com preços errados perde a confiança do utilizador. As restantes são propostas para a Fase 1, a discutir com o ChatGPT, e nenhuma muda a Fase 0.

**Lições propostas para a Fase 1**

| # | Lição | Base |
| --- | --- | --- |
| L1 | Mostrar sempre a idade do preço e separar “indicativo” de “confirmado” | Q1, Q2 |
| L2 | Testar cada link de reserva e ter alternativa quando falha | Q3, F10 |
| L3 | Manter uma interface simples, centrada na família (2 adultos + bebé, preço total) | Q4 |
| L4 | Decidir se as cores do mapa são relativas às outras rotas (como no Great Escape) ou por escalões fixos em euros | F1 |
| L5 | Filtros candidatos: orçamento, só directos, região; clima e popularidade só se houver fonte gratuita | F3 |
| L6 | Juntar ao mapa uma lista por continente ou país | F4 |
| L7 | Deixar de fora, no início, a informação turística e de vistos: exige outras fontes e manutenção | F6 |

**Lacunas por verificar na revisão manual**

Estas são as perguntas a que as fontes públicas não respondem, ou respondem com informação antiga. São a base da grelha de validação do documento de instruções.

| # | Pergunta | Itens relacionados |
| --- | --- | --- |
| G1 | O site ainda funciona e aceita FAO, LIS e SVQ como origem? | F2, F12 |
| G2 | Que filtros existem hoje? | F3, F7 |
| G3 | Como funciona a escala de cores do mapa? | F1 |
| G4 | Há pesquisa só de ida e datas flexíveis na versão grátis? | F5, F9 |
| G5 | Onde está hoje a fronteira entre a versão grátis e a paga, e quanto custa a subscrição? | F9, Q5 |
| G6 | É possível indicar passageiros e bebés? | F13 |
| G7 | O site mostra a idade dos preços ou avisa que são estimativas? | Q1, Q2 |
| G8 | Para que site encaminha a reserva, e o link abre o voo certo? | F10, Q3 |
| G9 | Em que moeda aparecem os preços para Portugal? | — |

## Fontes

Consultadas a 4 de Novembro de 2026. As fontes de imprensa marcadas com \* foram lidas só pelo excerto do motor de pesquisa, não pela página completa.

- [Great Escape — página “Sobre”](https://webdev.greatescape.co/about)
- [Great Escape — Termos e condições](https://webdev.greatescape.co/terms)
- [App Store — Escape](https://apps.apple.com/us/app/id1511294506)\*
- [Google Play — Escape](https://play.google.com/store/apps/details?id=com.greatescape&hl=en)\*
- [APKCombo — Escape 1.0.5](https://apkcombo.com/escape-explore-flight-deals/com.greatescape/)\*
- [Product Hunt — Escape](https://www.producthunt.com/products/escape-travel-inspiration-by-price)\*
- [Product Hunt — avaliações](https://www.producthunt.com/posts/escape-3/reviews)\*
- [LinkedIn — GREAT ESCAPE](https://www.linkedin.com/company/greatescapeco)\*
- [Tracxn — Great Escape](https://tracxn.com/d/companies/great-escape/__6TPgSSFxiEhWxlg1Zdjq4NFJ24GVnguSeQnwwNLUdH0)\*
- [FlyerTalk — Great Escape Review](https://www.flyertalk.com/?p=59962)\*
- [The Points Guy — New Escape travel tool](https://thepointsguy.com/news/new-escape-travel-tool)\*
- [Lonely Planet — Escape flight search engine map](https://www.lonelyplanet.com/news/escape-flight-search-engine-map)\*
- [Matador Network — Cheap flight search tool](https://matadornetwork.com/read/cheap-flight-search-tool/amp/)\*
- [CardRates — Escape offers flight bargains](https://www.cardrates.com/news/escape-offers-flight-bargains-and-flexible-scheduling/)\*
- [Mental Floss — Cheapest flights online tool](https://www.mentalfloss.com/transportation/planes-airports/cheapest-flights-online-tool)\*
- [TravelPulse — New tool finds cheapest flights](https://www.travelpulse.com/news/airlines/new-tool-finds-you-the-cheapest-flights-to-every-city-in-the-world.html)\*
- [My Modern Met — Cheap flights Escape MIT](https://mymodernmet.com/cheap-flights-escape-mit/)\*
- [Reddit (r/dataisbeautiful) — anúncio de 2019 e comentários](https://axebps-redlib.hf.space/r/dataisbeautiful/comments/b5qpfw/update_we_created_a_tool_to_visualise_the/ejf697h)\*
