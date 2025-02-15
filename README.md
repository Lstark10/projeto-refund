# Projeto de Solicitação de Reembolso

Este é um projeto simples de solicitação de reembolso desenvolvido para praticar e aplicar conceitos de JavaScript. O objetivo é permitir que os usuários adicionem despesas, visualizem a lista de despesas e o valor total, e removam despesas da lista.

## Estrutura do Projeto

O projeto é composto pelos seguintes arquivos:

- `index.html`: Contém a estrutura HTML da página.
- `styles.css`: Contém os estilos CSS para a página.
- `script.js`: Contém o código JavaScript para manipulação do DOM e lógica do projeto.
- `img`: Pasta que contém os ícones SVG utilizados no projeto.

## Funcionalidades

### Adicionar Despesa

O formulário permite que o usuário adicione uma nova despesa preenchendo os campos de nome, categoria e valor da despesa. Ao submeter o formulário, a despesa é adicionada à lista de despesas.

### Atualizar Totais

A função `updateTotals` é responsável por atualizar a quantidade de despesas e o valor total das despesas. Ela percorre todos os itens da lista, soma os valores e atualiza o total exibido na interface.

### Remover Despesa

Os itens da lista de despesas possuem um ícone de remoção. Ao clicar no ícone, a despesa correspondente é removida da lista e os totais são atualizados.
