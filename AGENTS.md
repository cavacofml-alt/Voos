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
3. O merge só acontece com a aprovação dos dois assistentes registada no pull request **e** com a aprovação final do utilizador.
4. Discordâncias ficam escritas no pull request, com os argumentos de cada lado. Quem decide é o utilizador; nunca se contornam.
5. Cada decisão relevante é registada em `docs/DECISOES.md`, no mesmo pull request.

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
| greatescape.co | Proibido: acesso automático (incluindo agentes que controlam o navegador), uso das APIs internas, cópia de conteúdo ou preços. Os termos do site exigem consentimento escrito |
| Google Flights | Proibido o scraping, directo ou através de intermediários como a SerpApi |
| easyJet | Sem qualquer automação; só confirmação manual |
| Ryanair | Só confirmação manual na Fase 0. A ausência de uma proibição expressa para uso não comercial não é autorização geral para automatizar |
| Hermes (API não oficial da Ryanair) | Não é dependência do projecto |
| Travelpayouts, Ignav | Fontes aprovadas, dentro dos respectivos termos e limites |

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
