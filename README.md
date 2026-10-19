<div align="center">

# Monitor de Margem e Estoque

**Entenda a margem dos seus produtos. Antecipe a falta de estoque. Priorize a reposição.**

Projeto de portfólio em dados e automação · Futuro Agora · GilisP

**Status: planejamento e dados de exemplo prontos — aplicação ainda não implementada.**

[Objetivo](#objetivo-final) · [Demonstração planejada](#um-exemplo-concreto) · [Roadmap](docs/roadmap.md) · [Contrato de dados](docs/dados.md) · [Guia para o agente](docs/prompt-agente.md)

</div>

---

## Objetivo final

Construir uma ferramenta web em português para pequenos vendedores transformarem planilhas de produtos, vendas e estoque em decisões de negócio explicáveis.

Ao final, o usuário poderá importar três arquivos CSV, corrigir problemas de qualidade, consultar margem de contribuição por produto, estimar cobertura do estoque e exportar uma lista priorizada de reposição. Cada indicador deverá mostrar seus critérios e cada recomendação deverá explicar o motivo.

O projeto pretende demonstrar um fluxo completo de trabalho com dados: definição de regras, importação, validação, transformação, cálculo, visualização e comunicação das limitações. Não há resultados de uso real ou ganhos de produtividade medidos nesta fase.

## O problema

Uma loja pode vender bastante e ainda ter produtos com margem negativa. Também pode manter itens parados enquanto seus produtos mais vendidos ficam sem estoque. Planilhas separadas dificultam perceber essas situações a tempo.

A ferramenta será organizada em torno de três perguntas:

| Pergunta do vendedor | Resposta planejada |
| --- | --- |
| Quais produtos deixam margem? | Receita, custos variáveis, margem em reais e percentual |
| Quais produtos podem acabar? | Cobertura estimada e comparação com o prazo de reposição |
| O que devo revisar primeiro? | Lista de prioridades, compra sugerida e alertas de margem negativa |

## Experiência planejada

1. **Importar:** baixar os modelos ou carregar a loja fictícia incluída no repositório.
2. **Validar:** receber erros com arquivo, linha, campo e orientação de correção.
3. **Analisar:** escolher o período e consultar indicadores e filtros por categoria e produto.
4. **Decidir:** revisar prioridades e entender a regra por trás de cada sugestão.
5. **Exportar:** baixar o relatório com os parâmetros usados na análise.

### Telas previstas

- **Importação:** três arquivos, modelos para download e relatório de validação.
- **Visão geral:** receita, margem de contribuição, unidades vendidas e alertas.
- **Produtos:** tabela filtrável e comparação de margem e cobertura.
- **Reposição:** prioridades, quantidades sugeridas e justificativas.

## Um exemplo concreto

A loja de acessórios incluída em `dados_exemplo/` é **inteiramente sintética**. Para setembro de 2026, com três dias de segurança, as referências manuais incluem:

| Produto | Situação | Resultado esperado |
| --- | --- | --- |
| Cabo USB | 10 unidades em estoque e média de 2 vendas/dia | Cobertura de 5 dias; sugestão de 10 unidades |
| Capa | Margem negativa e cobertura de 4 dias | Alerta de margem e prioridade urgente |
| Fone | Estoque zero e vendas recentes | Ruptura; sugestão de 13 unidades |
| Suporte | Sem vendas no período | Cobertura indefinida e revisão manual |

**Referência para conferir a futura implementação:** receita de R$ 4.650,00, margem de contribuição de R$ 1.530,00, 135 unidades vendidas e compra sugerida total de 25 unidades. Esses números são cálculos esperados da base fictícia, não resultados produzidos por uma aplicação pronta. A derivação completa está no [contrato de dados](docs/dados.md).

## Escopo da primeira versão

| Incluído | Evoluções posteriores |
| --- | --- |
| Importação de CSV e dados demonstrativos | Integração autorizada com marketplace |
| Validação de estrutura e conteúdo | Histórico persistido em banco de dados |
| Margem com custo histórico da venda | Tratamento de devoluções e cancelamentos |
| Cobertura e reposição explicáveis | Sazonalidade e pedidos de compra em trânsito |
| Interface em português e exportação CSV | Integração com um projeto separado de faturamento |

Compras automáticas e emissão fiscal não fazem parte desta primeira versão.

## Como os indicadores serão calculados

```text
Custo das mercadorias = soma(quantidade × custo unitário histórico)
Margem de contribuição = receita − custo das mercadorias − taxas − frete
Margem percentual = margem de contribuição ÷ receita × 100
Média diária = unidades vendidas ÷ dias corridos do período
Cobertura = estoque disponível ÷ média diária
Estoque-alvo = média diária × (prazo de reposição + dias de segurança)
Compra sugerida = teto(máximo(0, estoque-alvo − estoque disponível))
```

Receita zero gera margem percentual indefinida. Sem vendas, a cobertura é indefinida e a compra sugerida é zero, com orientação de revisão manual. O período inclui dias sem vendas. O saldo é informado pela loja e não deve sofrer uma nova subtração das vendas importadas.

**Margem de contribuição não é lucro líquido.** A análise depende dos custos informados, não inclui despesas fixas e não prevê automaticamente mudanças de demanda. Valores financeiros devem usar precisão decimal. Consulte as regras completas em [docs/dados.md](docs/dados.md).

## Organização do repositório

```text
monitor-margem-estoque/
├── README.md                 # Apresentação e objetivo final
├── AGENTS.md                 # Orientações para quem implementar
├── .gitignore                # Proteção de dados locais e credenciais
├── .gitattributes            # Padronização de arquivos de texto
├── modelos/                  # CSVs vazios com cabeçalhos oficiais
├── dados_exemplo/            # Loja fictícia para demonstração
└── docs/
    ├── dados.md              # Contrato, fórmulas e referências manuais
    ├── arquitetura.md        # Separação de responsabilidades
    ├── roadmap.md            # Etapas e critérios de aceite
    └── prompt-agente.md      # Instrução pronta para iniciar o código
```

Os módulos Python, testes e interface serão criados durante a implementação, conforme a [arquitetura planejada](docs/arquitetura.md).

## Como começar

Neste momento, o repositório contém especificação e dados; **ainda não existe comando para executar a aplicação**.

```bash
git clone https://github.com/GilisP/monitor-margem-estoque.git
cd monitor-margem-estoque
```

Leia o [roadmap](docs/roadmap.md) e o [contrato de dados](docs/dados.md). Para desenvolver com um agente de IA, utilize o [prompt de implementação](docs/prompt-agente.md).

O código será desenvolvido em Python, com cálculos separados da interface. A biblioteca de interface será escolhida na etapa correspondente, registrando a decisão. As instruções de instalação e execução serão adicionadas quando houver código funcional.

## Qualidade e conclusão

A primeira versão estará concluída quando outra pessoa conseguir executar a ferramenta seguindo o README, carregar a base fictícia, obter os resultados de referência, entender erros de importação e exportar uma análise coerente com os filtros selecionados. Os testes deverão cobrir regras de negócio e casos extremos, conforme o roadmap.

## Dados e publicação

Somente dados sintéticos e modelos devem ser versionados. Dados reais, arquivos de clientes, segredos e credenciais não pertencem ao repositório. A publicação deste planejamento não representa uma aplicação disponível em produção.

## Autor

[GilisP](https://github.com/GilisP) · Projeto **Futuro Agora**
