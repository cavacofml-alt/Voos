# Revisão do Great Escape no computador — Instruções

Nov 4, 2026 · @Zé Manel

## Objectivo

Este documento guia uma revisão de cerca de 20 minutos ao site greatescape.co, feita pelo utilizador à mão, para confirmar ou corrigir o que as fontes públicas dizem no documento “Great Escape — Referência para a Fase 1”. O resultado é a grelha de validação preenchida.

| Quem | Papel |
| --- | --- |
| Utilizador | Usa o site como qualquer visitante e preenche a grelha por palavras próprias |
| Claude (no computador) | Ajuda a preencher e a organizar a grelha a partir das notas do utilizador; não navega no site |
| Claude (esta conversa) | Compara a grelha com o documento de referência e assinala confirmações e contradições |
| ChatGPT | Revê a comparação e as lições propostas antes de entrarem no repositório |

A revisão é manual porque os termos do Great Escape proíbem o acesso por meios automáticos, incluindo agentes que controlam um navegador.

## Ferramentas e regra de colaboração

O projecto conta com as ferramentas dos dois assistentes. Cada um deve propor a ferramenta mais adequada a cada tarefa, pedir ao outro que use as suas quando forem melhores, e questionar as escolhas do outro. Esta secção serve também para o ChatGPT saber o que o Claude tem disponível.

**Ferramentas do Claude disponíveis no projecto**

| Ferramenta | O que faz | Uso no projecto |
| --- | --- | --- |
| Pesquisa e leitura web | Pesquisa na internet e lê páginas públicas como texto | Verificar documentação de APIs e termos de utilização |
| Navegador integrado (app Claude no computador) | Navegador próprio que o Claude controla: abre páginas, clica e preenche formulários | Só em sites cujos termos o permitam; nunca no greatescape.co nem na easyJet; não usado na Ryanair na Fase 0 |
| Claude in Chrome | Controla o Chrome do utilizador, com as suas sessões iniciadas | Mesmas restrições do navegador integrado |
| Utilização do computador | Vê o ecrã e controla aplicações do computador do utilizador | Não previsto nesta fase |
| GitHub | Liga repositórios à sessão, lê e propõe alterações por pull request | Repositório do projecto, depois de criado |
| Supabase | Cria tabelas, corre SQL e funções na conta do utilizador | Base de dados do teste, só após aprovação |
| Render | Gere serviços e tarefas na conta do utilizador | Alternativa de alojamento, se necessária |
| Context7 | Documentação actualizada de bibliotecas de programação | Na fase de código |
| Tarefas agendadas | Execuções automáticas em horário fixo | Recolha diária, só após aprovação |
| Documentos partilháveis | Documentos como este, com edição e comentários | Especificações antes de irem para o repositório |

**Regra de colaboração entre Claude e ChatGPT**

1. Antes de cada tarefa, quem a executa diz que ferramenta vai usar e porquê. O outro pode contestar e propor outra.
2. Se o outro assistente tiver uma ferramenta melhor para a tarefa, pede-se-lhe que a use. O Claude pede ao ChatGPT, por exemplo, análises com execução de código ou pesquisas aprofundadas; o ChatGPT pede ao Claude, por exemplo, operações no Supabase, no GitHub ou no navegador.
3. Factos que decidem o projecto, como parâmetros de API ou termos de utilização, são verificados pelos dois, cada um com as suas ferramentas, de forma independente.
4. Nenhuma ferramenta é usada contra os termos de utilização de um site, mesmo que tecnicamente funcione.
5. Esta regra passa para o ficheiro de regras do repositório no primeiro pull request.

## Regras da revisão

A revisão observa como o site funciona; não recolhe os seus dados. As regras seguem os termos do Great Escape (em vigor desde 1 de Setembro de 2020).

| Permitido | Não permitido |
| --- | --- |
| Usar o site normalmente, como qualquer visitante | Pedir ao Claude (ou a outro agente) que navegue no site |
| Descrever funcionalidades por palavras próprias | Copiar preços, listas de destinos ou textos do site |
| Indicar se algo existe ou não (sim/não) | Fazer capturas de ecrã ou gravações do site |
| Criar uma conta gratuita, se o site a pedir | Subscrever só para testar, sem o utilizador decidir que quer |
| Abrir o link de reserva uma vez para ver para onde leva | Abrir as ferramentas de programador do navegador para ver o tráfego de rede |
| Ler as páginas públicas de ajuda, “Sobre” e termos | Tentar contornar a parte paga |

Se uma funcionalidade só existir na versão paga, regista-se isso e não se vai mais longe.

## Procedimento

Nove passos, um por pergunta da grelha; cada resposta vai para a coluna “Observado”, em poucas palavras.

1. **G1 — Funcionamento e origens.** Abrir greatescape.co num navegador normal. Pesquisar a partir de Faro, depois de Lisboa e de Sevilha, com datas daqui a cerca de dois meses e destino em branco. Registar se cada origem é aceite e se o mapa carrega.
2. **G2 — Filtros.** Listar os filtros visíveis e assinalar os que pedem a versão paga.
3. **G3 — Cores do mapa.** Descrever a escala: relativa às outras rotas ou por valores fixos? Há legenda?
4. **G4 — Só ida e datas flexíveis.** Verificar se existem na versão grátis.
5. **G5 — Grátis e pago.** Registar o que está bloqueado e o preço da subscrição, se for mostrado, sem subscrever.
6. **G6 — Passageiros.** Verificar se se pode indicar o número de adultos e bebés.
7. **G7 — Idade dos preços.** Procurar qualquer indicação de quando o preço foi encontrado ou aviso de que é estimado.
8. **G8 — Reserva.** Clicar uma vez no botão de reserva de um destino e registar para que site leva e se o voo aparece seleccionado. Não reservar.
9. **G9 — Moeda.** Registar a moeda dos preços e se pode ser mudada.

Se o utilizador estiver a usar a app Claude no computador, pode ditar as observações ao Claude, que preenche a grelha. O Claude não abre o site.

## Grelha de validação

A coluna “Fontes públicas” resume o documento de referência; o utilizador preenche “Observado” e escolhe o estado. “Novo” serve para algo que as fontes não referem.

| # | Pergunta | Fontes públicas (2019–2024) | Observado | Estado |
| --- | --- | --- | --- | --- |
| G1 | O site funciona e aceita FAO, LIS e SVQ? | Pesquisa por origem e datas com destino em branco (2019) |  | Não verificado |
| G2 | Que filtros existem? | Orçamento, directos, vistos, popularidade, clima, região; exclusão de companhias numa fonte |  | Não verificado |
| G3 | Como funciona a escala de cores? | Verde a vermelho, relativa às outras rotas das mesmas datas |  | Não verificado |
| G4 | Só ida e datas flexíveis na versão grátis? | Só ida desde 2019; datas flexíveis prometidas em 2019; datas específicas pagas segundo um utilizador |  | Não verificado |
| G5 | O que é pago e quanto custa? | Subscrição sem período grátis; mapa ao vivo passou a pago; preço desconhecido |  | Não verificado |
| G6 | Pode indicar-se adultos e bebés? | Nenhuma fonte o refere |  | Não verificado |
| G7 | Mostra a idade dos preços ou avisa que são estimados? | Nenhuma fonte o refere; queixas de preços errados |  | Não verificado |
| G8 | Para onde leva a reserva e o voo vem seleccionado? | Kiwi ou Skyscanner (2019); na app iOS, Kayak com erro |  | Não verificado |
| G9 | Em que moeda aparecem os preços? | Nenhuma fonte o refere |  | Não verificado |

## Alternativa: pedir autorização

Se se quiser que o Claude navegue no site ou que se façam capturas de ecrã, é preciso consentimento escrito prévio da empresa. O pedido pode ser enviado para team@greatescape.co, em inglês. Sem resposta afirmativa por escrito, mantém-se a revisão manual.

```
Subject: Permission request – personal, non-commercial product review

Hello Escape team,

I am building a small personal tool, for my own family's use only, to find
cheap flights from Faro, Lisbon and Seville. I would like to review how
Escape works (filters, map, booking flow) as design inspiration.

Your Terms of Service require prior written consent for automated access
and for copying material. May I use an AI browsing assistant to navigate
the public site for about 30 minutes, and take a few screenshots for
personal reference only? No prices or data would be stored, reused or
published, and nothing would be used commercially.

Thank you,
<nome>
```

## Depois da revisão

1. O utilizador traz a grelha preenchida para a conversa com o Claude.
2. O Claude compara cada linha com o documento de referência e actualiza o estado (confirma, contradiz, novo) e as lições L1 a L7 afectadas.
3. O ChatGPT revê a comparação e as lições, com as suas próprias ferramentas, e pode pedir verificações adicionais ao Claude.
4. A versão acordada entra no repositório como referência da Fase 1, por pull request, sem alterar a Fase 0.
