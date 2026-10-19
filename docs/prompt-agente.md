# Prompt para o agente de implementação

Copie o texto abaixo para o agente que terá acesso ao repositório.

---

Você será meu parceiro de implementação do projeto Monitor de Margem e Reposição de Estoque, do portfólio Futuro Agora.

Repositório: https://github.com/GilisP/monitor-margem-estoque

## Objetivo

Construir uma ferramenta web local em português para pequenos vendedores importarem produtos, vendas e estoque em CSV, verificarem os dados, analisarem margem de contribuição e cobertura e exportarem prioridades de reposição explicáveis.

## Contexto e materiais

O repositório já contém o planejamento, modelos e uma loja inteiramente sintética. Ainda não há aplicação executável. Leia integralmente AGENTS.md, README.md, docs/dados.md, docs/arquitetura.md e docs/roadmap.md e inspecione os CSVs de modelos/ e dados_exemplo/ antes de implementar. Preserve o trabalho existente.

## Tarefa

Implemente o MVP em Python em etapas verificáveis, começando pela importação e validação. Depois implemente indicadores, reposição, interface e exportação. Separe as regras de negócio da interface. Não se limite a sugerir um plano: produza o código e execute as verificações disponíveis.

Explique decisões em português para que eu aprenda e consiga apresentar o projeto em uma entrevista. Não presuma meu nível nem bloqueie trabalho independente por perguntas opcionais. Escolha soluções simples; consulte a documentação oficial atualizada quando selecionar bibliotecas, versões e APIs. Registre dependências usadas e a versão de Python suportada.

## Regras essenciais

- Siga exatamente o contrato de dados e relate conflitos antes de alterá-lo.
- Use custo histórico da venda, preserve SKU como texto e nunca substitua dados obrigatórios ausentes por zero.
- Use precisão decimal para dinheiro e trate com cuidado o arredondamento para cima das quantidades de compra, sem erro nas fronteiras.
- Calcule margem percentual agregada pelos totais, não pela média de percentuais.
- Considere dias corridos inclusivos; valide os parâmetros e a data do estoque.
- Receita zero e cobertura sem vendas são indefinidas; sem vendas, compra sugerida zero com explicação de revisão manual.
- Não desconte as vendas novamente do saldo informado.
- Mostre erros com arquivo, linha quando aplicável, campo, motivo e orientação.
- Demonstre alertas de margem negativa mesmo quando houver sugestão de compra.
- Mantenha filtros, totais exibidos e exportação coerentes, com parâmetros identificáveis.
- Inclua testes relevantes dos cálculos, validações e casos extremos do roadmap, além de uma verificação do fluxo completo.
- Use apenas dados sintéticos e não adicione segredos ao Git.
- Não inclua emissão fiscal, integrações de marketplace, compras automáticas ou banco de dados nesta primeira versão.
- Não faça publicação, push, deploy ou outras ações externas sem minha autorização correspondente.

## Critério de conclusão

Eu devo conseguir seguir o README em ambiente limpo, executar a aplicação, carregar a base fictícia, corrigir erros de importação, consultar resultados e exportar a análise. Para setembro de 2026 e segurança de 3 dias, conferir receita de R$ 4.650,00, margem de R$ 1.530,00, 135 unidades vendidas e 25 unidades de compra sugerida. A ordem de prioridade deve ser FONE-005, CAPA-003, CABO-001, CARG-002 e SUP-004. Confira também todas as referências por produto em docs/dados.md.

Atualize README e roadmap de acordo com o que realmente foi entregue, sem apresentar planejamento como funcionalidade pronta. Ao terminar, informe resultado, evidências e verificações executadas, limitações, comandos para executar e próximo passo. Não invente ganhos de produtividade ou resultados reais.
