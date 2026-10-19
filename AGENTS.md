# Orientações de implementação

## Contexto

Este repositório é o Monitor de Margem e Reposição de Estoque do projeto Futuro Agora, de GilisP. É um projeto de aprendizado e portfólio para pequenos vendedores. A documentação e os dados sintéticos precedem a aplicação; não descreva funcionalidades planejadas como implementadas.

## Leitura inicial

Leia integralmente README.md, docs/dados.md, docs/roadmap.md e docs/arquitetura.md antes de alterar código. O contrato de dados é a referência para campos e cálculos. Relate conflitos antes de mudar o contrato ou os resultados esperados.

## Trabalho

- Comunique-se em português e explique as decisões para apoiar o aprendizado.
- Implemente em Python, com importação, validação, indicadores e apresentação separados.
- Comece pela etapa 2 do roadmap; preserve a etapa 1 e os arquivos existentes.
- Não adicione emissão fiscal, integrações externas, compras automáticas ou autenticação ao MVP.
- Use Decimal para dinheiro e evite arredondamentos intermediários que alterem decisões de reposição.
- Preserve SKU como texto, incluindo zeros iniciais.
- Dados inválidos devem produzir erros acionáveis; não preencha campos obrigatórios ausentes com zero nem calcule resultados parciais sem indicar o problema.
- Não adicione comentários inline ou dependências desnecessárias.
- Verifique versões e APIs na documentação oficial ao escolher bibliotecas. Registre dependências realmente utilizadas e comandos reproduzíveis.
- Teste regras de negócio e limites reais. Não crie testes vazios ou funcionalidades apenas para satisfazer um teste.
- Atualize README e roadmap conforme o que foi efetivamente entregue.
- Não invente usuários, experiência profissional, métricas de impacto ou resultados de uso real.

## Dados e ações externas

Use apenas a base sintética versionada para demonstrações. Nunca versione dados pessoais, credenciais ou arquivos reais da loja. Não faça deploy, emita documentos fiscais, altere anúncios, envie mensagens ou publique novas alterações sem autorização do usuário para a ação correspondente.

## Relato de entrega

Indique resultado, evidências e verificações executadas, dificuldades ou limitações e próximo passo. Inclua comandos de execução quando houver aplicação funcional.
