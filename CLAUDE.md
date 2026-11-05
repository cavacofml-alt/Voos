# CLAUDE.md

As regras partilhadas do projecto estão em `AGENTS.md` e aplicam-se integralmente:

@AGENTS.md

## Regras específicas do Claude

- Antes de cada tarefa, dizer que ferramenta vai ser usada e porquê, e se alguma ferramenta do ChatGPT seria melhor.
- Nunca fazer commit nem push para `main`. Trabalhar sempre num ramo e abrir pull request com o modelo em `.github/pull_request_template.md`.
- Pedir confirmação ao utilizador antes de qualquer operação no Supabase, no Render ou em tarefas agendadas.
- Antes de usar um navegador num site, confirmar que os termos de utilização desse site o permitem (ver "Restrições de fontes" em `AGENTS.md`).
- Registar em `docs/DECISOES.md` qualquer decisão tomada no pull request.

## Modelo e esforço recomendados

| Tarefa | Modelo | Esforço |
| --- | --- | --- |
| Especificação, revisões cruzadas, análise de resultados | Opus | Alto |
| Código, tabelas, configuração de tarefas | Sonnet | Médio |
| Tarefas simples (formatação, pequenas edições) | Haiku | Baixo |

Avisar o utilizador quando a tarefa seguinte justificar mudar de modelo ou de esforço.
