# Regras do projecto — Monitor de Voos

Regras partilhadas por todos os assistentes que trabalham neste repositório (Claude, ChatGPT e outros). O `CLAUDE.md` importa este ficheiro.

## Objectivo

Ferramenta pessoal para encontrar voos baratos para uma família de 2 adultos + 1 bebé ao colo, a partir de Faro (FAO), Lisboa (LIS) e Sevilha (SVQ). Uso privado e não comercial. Custo zero, aceitando no máximo 1 a 2 € por mês se isso melhorar claramente a fiabilidade.

## Papéis

| Quem | Papel |
| --- | --- |
| Utilizador | Decide. Tem a aprovação final de qualquer alteração |
| Claude | Propõe, implementa e revê o trabalho do ChatGPT |
| ChatGPT | Propõe, implementa e revê o trabalho do Claude |

## Processo de alteração

1. Nenhuma alteração vai directamente para `main`. Tudo passa por pull request.
2. Quem propõe nunca revê o próprio trabalho: o outro assistente faz a revisão no pull request.
3. Registam-se no pull request três etapas separadas: (a) a confirmação do autor de que o pull request está completo; (b) a aprovação do revisor, que é sempre o outro assistente; (c) a aprovação final do utilizador. Quem propõe nunca marca a etapa (b).
4. O merge só acontece com as três etapas (a), (b) e (c) registadas no pull request.
5. Discordâncias ficam escritas no pull request, com os argumentos de cada lado. Quem decide é o utilizador; nunca se contornam.
6. Cada decisão relevante é registada em `docs/DECISOES.md`, no mesmo pull request.

## Crítica e verificação

- Cada assistente deve ser crítico do trabalho do outro. As objecções citam fontes: documentação oficial, termos de utilização ou dados.
- Os factos que decidem o projecto (parâmetros de API, termos de utilização, custos) são verificados pelos dois, cada um com as suas ferramentas, de forma independente.
- Os limiares de aprovação da Fase 0 não se alteram para facilitar uma aprovação. O que não puder ser medido declara-se **não mensurável**.
- Ausência de dados numa fonte nunca se interpreta como ausência de voos.

## Ferramentas e colaboração

1. Antes de cada tarefa, quem a executa diz que ferramenta vai usar e porquê. O outro pode contestar e propor outra.
2. Se o outro assistente tiver uma ferramenta melhor para a tarefa, pede-se-lhe que a use, e vice-versa.
   - Claude: pesquisa e leitura web, navegador (app no computador), GitHub, Supabase, Render, documentação de bibliotecas (Context7), tarefas agendadas.
   - ChatGPT: as ferramentas que tiver activas, por exemplo pesquisa web, análise com execução de código e acesso a este repositório.
3. Nenhuma ferramenta é usada contra os termos de utilização de um site, mesmo que tecnicamente funcione.

## Restrições de fontes

| Fonte | Regra |
| --- | --- |
| `*.greatescape.co` (incluindo subdomínios, como `webdev.greatescape.co`) | Proibido, para todos os assistentes e agentes: acesso automático, navegação automatizada (incluindo agentes que controlam o navegador), scraping, uso das APIs internas e cópia não autorizada de conteúdo ou preços. Os termos do site exigem consentimento escrito. A revisão do site é manual, feita pelo utilizador |
| Google Flights | Proibido o scraping, directo ou através de intermediários como a SerpApi |
| easyJet | Sem qualquer automação; só confirmação manual |
| Ryanair | Só confirmação manual na Fase 0. A ausência de uma proibição expressa para uso não comercial não é autorização geral para automatizar |
| Hermes (API não oficial da Ryanair) | Não é dependência do projecto |
| Travelpayouts, Ignav | Aprovadas para os usos descritos na Fase 0, sujeitas à confirmação dos termos, quotas e preços vigentes. Condições: [Travelpayouts — Aviasales Data API](https://support.travelpayouts.com/hc/en-us/articles/203956163); Ignav: [Cheapest Dates](https://ignav.com/docs/cheapest-dates), [One-Way Fares](https://ignav.com/docs/one-way), [Booking Links](https://ignav.com/docs/booking-links) e [Preços](https://ignav.com/pricing). Ignav: 1.000 pedidos gratuitos, uma só vez, e depois 2 USD por 1.000 pedidos bem-sucedidos, em que só as respostas HTTP 200 são cobradas (fonte: [página de preços da Ignav](https://ignav.com/pricing), verificada pelo Claude da conversa web a 5 de Novembro de 2026). Limites da Travelpayouts: a confirmar no Dia 0 |

## Segredos

Tokens e chaves (Travelpayouts, Ignav, Supabase) nunca entram no repositório. Usam-se variáveis de ambiente ou os segredos do serviço de alojamento. O ficheiro `.env` está no `.gitignore`.

## Fase actual

**Fase 0 — Validação de dados.** A especificação aprovada está em `docs/fase-0-validacao-dados.md`.

Não se cria código, tabelas no Supabase nem tarefas agendadas antes de:

1. a revisão final do ChatGPT no pull request da especificação;
2. a aprovação final do utilizador;
3. a criação das contas na Travelpayouts e na Ignav.

## Idioma

Documentação, mensagens de commit, pull requests e comentários em português europeu.
