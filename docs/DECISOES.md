# Registo de decisões

Cada decisão relevante do projecto, com quem a propôs, quem a reviu e o estado. As novas decisões acrescentam-se no fim, no mesmo pull request em que são tomadas.

**Estados:** Em vigor · Pendente de revisão · Substituída (indicar por qual)

| ID | Data | Decisão | Proposta | Revista | Estado |
| --- | --- | --- | --- | --- | --- |
| D-001 | 2026-11-04 | Ferramenta pessoal e não comercial, para 2 adultos + 1 bebé ao colo, com partida de FAO, LIS e SVQ. Custo zero, aceitando até 1–2 € por mês se melhorar claramente a fiabilidade | Utilizador | — | Em vigor |
| D-002 | 2026-11-04 | Não usar as APIs internas nem os dados do greatescape.co, nem contornar a sua subscrição | Claude | ChatGPT | Em vigor |
| D-003 | 2026-11-04 | Não usar scraping do Google Flights, directo ou através da SerpApi | Claude | ChatGPT | Em vigor |
| D-004 | 2026-11-04 | Excluir Amadeus Self-Service (encerrado a 17/07/2026), Skyscanner e Kiwi (acesso só para parceiros de grande volume) e Duffel (modo gratuito só com dados fictícios) | Claude | ChatGPT | Em vigor |
| D-005 | 2026-11-04 | Arquitectura: descoberta com a Travelpayouts, confirmação com a Ignav, verificação final manual no site da companhia | Claude (Travelpayouts) e ChatGPT (Ignav) | Ambos | Em vigor |
| D-006 | 2026-11-04 | A confirmação final faz-se por link para o site da companhia, nunca por scraping | Claude | ChatGPT (Gemini consultado, sem papel de revisor) | Em vigor |
| D-007 | 2026-11-04 | O Hermes (API não oficial da Ryanair) não é dependência do projecto | ChatGPT | Claude | Em vigor |
| D-008 | 2026-11-04 | easyJet: sem qualquer automação. Ryanair: só confirmação manual na Fase 0, com formulação prudente sobre automação pessoal | ChatGPT (easyJet), Claude (Ryanair, corrigido pelo ChatGPT) | Ambos | Em vigor |
| D-009 | 2026-11-04 | Especificação da Fase 0, versão 4.1: métricas, células de decisão e limiares fixados antes do Dia 1; orçamento de 353 pedidos à Ignav | Claude | ChatGPT: quatro rondas de revisão técnica, condicionadas, que não autorizaram o Dia 0 nem a implementação. Aprovação final: pendente | Pendente de revisão final no pull request |
| D-010 | 2026-11-04 | Regra geral para todos os assistentes e agentes: ninguém acede ao greatescape.co nem aos seus subdomínios por meios automáticos, nem navega, faz scraping, usa as APIs internas ou copia conteúdo sem consentimento escrito, porque os termos o proíbem. A revisão do site é manual, feita pelo utilizador. Proveniência da consulta pontual de 4 de Novembro de 2026: ver `docs/referencia-great-escape.md` | Claude | — | Pendente de revisão |
| D-011 | 2026-11-04 | Regra de colaboração: cada assistente propõe e justifica a ferramenta, pede ao outro que use as suas quando forem melhores, questiona as escolhas do outro; factos decisivos verificados pelos dois | Utilizador | — | Em vigor |
| D-012 | 2026-11-04 | Processo: nada se altera sem acordo dos dois assistentes e aprovação final do utilizador; quem propõe não revê | Utilizador | — | Em vigor |
| D-013 | 2026-11-05 | Great Escape: a referência regista a proveniência da leitura pontual das páginas «Sobre» e «Termos» (a 4 de Novembro de 2026, com a ferramenta de leitura web, antes de se conhecerem os termos, sem acesso posterior nem consentimento escrito). A informação dessas páginas fica identificada como consulta pontual, sem novas verificações automáticas. A restrição no `AGENTS.md` passa a abranger `*.greatescape.co` e todos os assistentes e agentes | Claude, a pedido do ChatGPT | — | Pendente de revisão |
| D-014 | 2026-11-05 | Aprovação de cada pull request em três etapas separadas: (a) confirmação do autor de que está completo; (b) aprovação do revisor, que é o outro assistente; (c) aprovação final do utilizador. Quem propõe nunca marca (b) | Claude, a pedido do ChatGPT | — | Pendente de revisão |
| D-015 | 2026-11-05 | Travelpayouts e Ignav são fontes aprovadas apenas para os usos descritos na Fase 0, sujeitas à confirmação dos termos, quotas e preços vigentes. Limites da Travelpayouts a confirmar no Dia 0 | Claude, a pedido do ChatGPT | — | Pendente de revisão |
| D-016 | 2026-11-05 | Nenhum assistente faz `force push` nem reescreve o histórico de um ramo sem autorização expressa do utilizador | Utilizador | — | Pendente de revisão |
| D-017 | 2026-11-05 | A especificação da Fase 0 publicada é a exportação directa da revisão 72 do documento partilhado (versão 4.1 com as quatro alterações finais). Não se cria outro número de versão | Claude, a pedido do ChatGPT | — | Pendente de revisão |
| D-018 | 2026-11-05 | A regra da Ignav sobre a reutilização de resultados durante 30 minutos fica registada como verificada só pelo ChatGPT, sem confirmação independente do Claude, até haver um URL exacto e uma segunda verificação | Claude, a pedido do ChatGPT | — | Pendente de revisão |
| D-019 | 2026-11-05 | Correcções menores sem efeito nas métricas: datas escritas em português, URLs do Google Play corrigidos, caixa da regra de colaboração marcada, «Fase 0 fechada» substituído por «especificação concluída, pendente da revisão final no pull request» | Claude | — | Pendente de revisão |
