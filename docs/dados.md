# Contrato de dados — versão 1

Todos os exemplos são sintéticos. CSV em UTF-8, separador vírgula, primeira linha com cabeçalhos exatos, datas ISO `AAAA-MM-DD`, valores monetários com ponto decimal e sem símbolo de moeda. Campos obrigatórios não podem ficar vazios. SKUs são textos e devem ser preservados, inclusive zeros iniciais.

## Produtos

| Campo | Tipo | Significado e regra |
| --- | --- | --- |
| sku | texto | Identificador único do produto, sem espaços nas extremidades |
| nome | texto | Nome obrigatório |
| categoria | texto | Categoria obrigatória |
| custo_unitario_atual | decimal | Custo atual em reais, maior ou igual a zero; não recalcula vendas antigas |

## Vendas

| Campo | Tipo | Significado e regra |
| --- | --- | --- |
| id_item_venda | texto | Identificador único da linha; repetição é erro |
| id_venda | texto | Identificador da transação; pode repetir em itens distintos |
| data | data | Dia da venda |
| sku | texto | Deve existir em produtos |
| quantidade | inteiro | Unidades vendidas, maior que zero |
| receita | decimal | Total deste item após descontos, antes de taxas e frete; maior ou igual a zero |
| custo_unitario_venda | decimal | Custo de uma unidade na data da venda; obrigatório e maior ou igual a zero |
| taxas | decimal | Total de taxas atribuído a este item, maior ou igual a zero |
| frete_vendedor | decimal | Total de frete pago pelo vendedor atribuído a este item, maior ou igual a zero |

Em vendas com vários itens, taxas e frete devem ser rateados antes da importação; não repetir o total da transação em cada linha. Devoluções e cancelamentos ficam fora deste modelo inicial e precisam de uma especificação posterior.

## Estoque

| Campo | Tipo | Significado e regra |
| --- | --- | --- |
| sku | texto | Uma linha por produto cadastrado; todos os produtos devem estar presentes |
| data_referencia | data | Data do saldo; todas as linhas devem usar a mesma data |
| quantidade_disponivel | inteiro | Saldo disponível para venda nessa data, maior ou igual a zero |
| prazo_reposicao_dias | inteiro | Prazo do fornecedor em dias corridos, maior ou igual a zero |

O saldo é um retrato informado pela loja, não é calculado subtraindo as vendas destes arquivos. Um SKU cadastrado sem vendas é válido; um SKU sem estoque é erro.

## Parâmetros da análise

Na demonstração: início `2026-09-01`, fim `2026-09-30`, saldo em `2026-09-30` e segurança de 3 dias. O intervalo inclui as duas datas: 30 dias corridos, inclusive dias sem vendas. A data final deve coincidir com a referência do estoque nesta primeira versão. Vendas fora do intervalo não entram nos indicadores e devem ter sua contagem informada.

## Fórmulas e prioridades

- Custo das mercadorias: soma de `quantidade × custo_unitario_venda` de cada item no período.
- Margem de contribuição: soma de receita menos custo das mercadorias, taxas e frete.
- Margem percentual: margem total / receita total × 100; receita zero resulta em percentual não definido, nunca zero automático.
- Média diária: unidades vendidas / dias corridos do intervalo.
- Cobertura: estoque disponível / média diária; sem vendas, cobertura não definida.
- Estoque-alvo: média diária × (prazo de reposição + segurança).
- Compra sugerida: arredondar para cima o máximo entre zero e estoque-alvo menos saldo.
- Prioridade: sem vendas → “sem demanda recente”; com vendas e saldo zero → “ruptura”; cobertura menor ou igual ao prazo → “urgente”; cobertura abaixo do prazo mais segurança → “atenção”; demais → “regular”. Ordenar ruptura, urgente, atenção, regular e sem demanda recente; em empate, menor cobertura e depois SKU.

Sem vendas recentes: compra sugerida zero, acompanhada da explicação “sem demanda recente; revisar manualmente”. Margem negativa deve gerar alerta, inclusive quando houver sugestão de reposição. Usar precisão decimal para dinheiro e arredondar somente a apresentação; quantidades de compra sempre para cima.

## Referências manuais da loja sintética

| SKU | Unidades | Receita | Custo mercadorias | Taxas | Frete | Margem | Margem % | Saldo | Média/dia | Cobertura | Prazo | Alvo | Compra | Prioridade |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| CABO-001 | 60 | 1200 | 600 | 120 | 60 | 420 | 35% | 10 | 2 | 5 dias | 7 | 20 | 10 | urgente |
| CARG-002 | 30 | 1800 | 900 | 180 | 90 | 630 | 35% | 30 | 1 | 30 dias | 5 | 8 | 0 | regular |
| CAPA-003 | 15 | 150 | 180 | 15 | 15 | -60 | -40% | 2 | 0,5 | 4 dias | 5 | 4 | 2 | urgente |
| SUP-004 | 0 | 0 | 0 | 0 | 0 | 0 | indefinida | 20 | 0 | indefinida | 7 | 0 | 0 | sem demanda recente |
| FONE-005 | 30 | 1500 | 750 | 150 | 60 | 540 | 36% | 0 | 1 | 0 dias | 10 | 13 | 13 | ruptura |

Totais: receita R$ 4.650,00; custo R$ 2.430,00; taxas R$ 465,00; frete R$ 225,00; margem R$ 1.530,00 (32,90%); 135 unidades e 4 produtos vendidos. Compra total sugerida: 25 unidades. Ordem: FONE-005, CAPA-003, CABO-001, CARG-002, SUP-004.

O cabo tem custo atual de R$ 11,00 e custo histórico de R$ 10,00: a diferença é intencional para verificar que o cadastro não altera a margem das vendas.

## Registro da etapa 1

Criados modelos e dados sintéticos para demanda alta, estoque regular, margem negativa, ausência de vendas e ruptura. Incluídos custo histórico e data do estoque para evitar análises sem referência temporal. Conferidos vínculos por SKU, campos, unicidade, consistência de datas e resultados manuais. Próximo passo: transformar estas regras em validações de importação.
