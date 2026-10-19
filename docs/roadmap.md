# Roadmap e critérios de aceite

## Estado atual

- [x] Objetivo e público definidos.
- [x] Contrato de dados e referências manuais documentados.
- [x] Modelos CSV e loja sintética disponíveis.
- [x] Repositório organizado para receber implementação.
- [ ] Importação e validação implementadas.
- [ ] Indicadores e reposição implementados.
- [ ] Interface e exportação implementadas.
- [ ] Aplicação testada e demonstrada.

## Etapa 1 — Dados e especificação

Entregue: três modelos CSV, base fictícia, definição dos campos e resultados esperados em dados.md. A documentação não substitui a futura validação automatizada.

## Etapa 2 — Importação confiável

Entregar leitura dos três CSV, estruturas de dados utilizáveis e erros acionáveis.

Critérios:

- Preservar SKUs textuais, inclusive zeros iniciais.
- Rejeitar cabeçalhos incorretos, campos obrigatórios vazios, datas e números inválidos, valores não finitos e quantidades incompatíveis.
- Detectar duplicidade de produto, item de venda e estoque; permitir id_venda repetido em itens diferentes.
- Detectar SKUs desconhecidos e produtos sem registro de estoque.
- Exigir uma única data de referência do estoque.
- Não alterar a base recebida ou substituir ausência por zero silenciosamente.
- Exibir arquivo, campo, motivo, correção e linha quando aplicável.
- Cobrir a base válida e exemplos inválidos por testes.

## Etapa 3 — Indicadores e reposição

Entregar funções independentes da interface para cálculo por produto e totais.

Critérios:

- Reproduzir todas as referências de dados.md, incluindo custo histórico e ordem de prioridade.
- Obter receita 4650, margem 1530, 135 unidades e compra sugerida total de 25 na demonstração.
- Validar período inclusivo e segurança inteira não negativa; exigir fim igual à referência do estoque.
- Informar quantas vendas foram excluídas pelo período.
- Manter produtos sem vendas no resultado.
- Tratar receita zero e cobertura indefinida explicitamente.
- Verificar margem negativa, ruptura, empates e fronteiras exatas de prioridade e arredondamento de compra.
- Não subtrair as vendas novamente do saldo informado.

## Etapa 4 — Interface e exportação

Entregar aplicação web local em português com importação, visão geral, produtos e reposição.

Critérios:

- Carregar dados sintéticos com ação clara e disponibilizar modelos.
- Não exibir resultados antigos como se pertencessem a uma nova importação inválida.
- Mostrar período, data do saldo, segurança e limitações da estimativa.
- Formatar reais, percentuais e valores indefinidos de modo compreensível.
- Filtrar por categoria/produto e explicar as sugestões.
- Exportar resultados coerentes com filtros e parâmetros, preservando SKUs e protegendo campos textuais contra interpretação como fórmula por planilhas.
- Conferir visualmente as telas e o fluxo de erro, além dos testes de negócio.

## Etapa 5 — Entrega para portfólio

Critérios:

- README com comandos reais e reproduzíveis de instalação, execução e testes.
- Dependências declaradas e ambiente limpo verificável.
- Capturas reais da ferramenta, identificadas como demonstração com dados sintéticos.
- Estudo de caso com problema, método, decisões, resultados verificáveis e limitações.
- Fluxo completo conferido: importar → validar → analisar → exportar.
- Sem credenciais, dados pessoais ou arquivos privados versionados.
- Atualizar status somente após evidência da funcionalidade.

## Evoluções fora do MVP

Histórico em banco de dados, integrações autorizadas com marketplaces, devoluções/cancelamentos e métodos de demanda mais elaborados. O assistente de faturamento é uma segunda iniciativa, não um requisito para concluir este projeto.
