# Projeto de Solicitação de Reembolso

Este é um projeto simples de solicitação de reembolso desenvolvido para praticar e aplicar conceitos fundamentais de JavaScript. O objetivo é permitir que os usuários adicionem despesas, visualizem a lista de despesas com o valor total e removam despesas da lista de forma dinâmica.

## 📁 Estrutura do Projeto

```
projeto-refund/
├── index.html          # Estrutura HTML da aplicação
├── styles.css          # Estilos CSS para a interface
├── script.js           # Lógica JavaScript principal
├── img/                # Ícones SVG das categorias
│   ├── food.svg        # Ícone para categoria alimentação
│   ├── transport.svg   # Ícone para categoria transporte
│   ├── health.svg      # Ícone para categoria saúde
│   └── remove.svg      # Ícone para remover despesas
└── README.md           # Documentação do projeto
```

## ⚡ Funcionalidades

### ➕ Adicionar Despesa
- Formulário com campos para nome da despesa, categoria e valor
- Formatação automática do valor em moeda brasileira (BRL)
- Validação dos dados inseridos
- Adição dinâmica à lista de despesas

### 🔄 Atualização Automática de Totais
- Contagem automática do número de despesas
- Cálculo e exibição do valor total das despesas
- Formatação adequada da moeda brasileira
- Atualização em tempo real

### 🗑️ Remover Despesa
- Ícone de remoção em cada item da lista
- Remoção individual de despesas
- Atualização automática dos totais após remoção

### 🧹 Limpeza do Formulário
- Limpeza automática dos campos após adicionar despesa
- Foco automático no primeiro campo para nova inserção

## 🛠️ Tecnologias Utilizadas

- **HTML5**: Estrutura semântica da aplicação
- **CSS3**: Estilização e layout responsivo
- **JavaScript ES6+**: Lógica de manipulação do DOM e funcionalidades

## 💻 Código JavaScript Detalhado

### Seleção de Elementos DOM

```javascript
// Elementos do formulário
const form = document.querySelector("form")
const amount = document.getElementById("amount")
const expense = document.getElementById("expense")
const category = document.getElementById("category")

// Elementos da lista de despesas
const expenseList = document.querySelector("ul")
const expenseQuantity = document.querySelector("aside header p span")
const expensesTotal = document.querySelector("aside header h2")
```

### Formatação de Moeda

```javascript
function formatCurrencyBRL(value) {
  value = value.toLocaleString("pt-BR", {
    style: "currency",
    currency: "BRL",
  })
  return value
}
```

**Funcionalidade**: Formata valores numéricos no padrão da moeda brasileira (R$).

### Formatação em Tempo Real do Input

```javascript
amount.oninput = () => {
  let value = amount.value.replace(/\D/g, "")
  value = Number(value) / 100
  amount.value = formatCurrencyBRL(value)
}
```

**Funcionalidade**: 
- Remove caracteres não numéricos
- Converte o valor para centavos
- Aplica formatação BRL em tempo real

### Captura e Processamento do Formulário

```javascript
form.onsubmit = (event) => {
  event.preventDefault()
  
  const newExpense = {
    id: new Date().getTime(),
    expense: expense.value,
    category_id: category.value,
    category_name: category.options[category.selectedIndex].text,
    amount: amount.value,
    created_at: new Date(),
  }
  
  expenseAdd(newExpense)
}
```

**Funcionalidade**:
- Previne o comportamento padrão do formulário
- Cria objeto com dados da despesa
- Gera ID único baseado em timestamp
- Chama função para adicionar despesa

### Adição de Despesas

```javascript
function expenseAdd(newExpense) {
  try {
    // Criação do elemento principal
    const expenseItem = document.createElement("li")
    expenseItem.classList.add("expense")

    // Ícone da categoria
    const expenseIcon = document.createElement("img")
    expenseIcon.setAttribute("src", `img/${newExpense.category_id}.svg`)
    expenseIcon.setAttribute("alt", newExpense.category_name)

    // Container de informações
    const expenseInfo = document.createElement("div")
    expenseInfo.classList.add("expense-info")

    // Nome da despesa
    const expenseName = document.createElement("strong")
    expenseName.textContent = newExpense.expense

    // Categoria da despesa
    const expenseCategory = document.createElement("span")
    expenseCategory.textContent = newExpense.category_name

    expenseInfo.append(expenseName, expenseCategory)

    // Valor da despesa
    const expenseAmount = document.createElement("span")
    expenseAmount.classList.add("expense-amount")
    expenseAmount.innerHTML = `<small>R$</small>${newExpense.amount.toUpperCase().replace("R$", "")}`

    // Ícone de remoção
    const removeIcon = document.createElement("img")
    removeIcon.classList.add("remove-icon")
    removeIcon.setAttribute("src", "img/remove.svg")
    removeIcon.setAttribute("alt", "remover")

    // Montagem do elemento
    expenseItem.append(expenseIcon, expenseInfo, expenseAmount, removeIcon)
    expenseList.append(expenseItem)

    // Limpeza e atualização
    formClear()
    updateTotals()
    
  } catch (error) {
    alert("Não foi possível atualizar a lista de despesas.")
    console.log(error)
  }
}
```

**Funcionalidade**:
- Cria elementos DOM dinamicamente
- Estrutura HTML completa para cada despesa
- Tratamento de erros com try/catch
- Limpeza automática do formulário

### Atualização de Totais

```javascript
function updateTotals() {
  try {
    const items = expenseList.children
    
    // Atualização da quantidade
    expenseQuantity.textContent = `${items.length} ${items.length > 1 ? "despesas" : "despesa"}`

    let total = 0

    // Cálculo do total
    for(let item = 0; item < items.length; item++) {
      const itemAmount = items[item].querySelector(".expense-amount")
      let value = itemAmount.textContent.replace(/[^\d,]/g, "").replace(",", ".")
      value = parseFloat(value)

      if (isNaN(value)) {
        return alert("Não foi possível calcular o total. O valor não parece ser um número.")
      }

      total += Number(value)
    }

    // Formatação e exibição do total
    const symbolBRL = document.createElement("small")
    symbolBRL.textContent = "R$"
    total = formatCurrencyBRL(total).toUpperCase().replace("R$", "")
    expensesTotal.innerHTML = ""
    expensesTotal.append(symbolBRL, total)
    
  } catch (error) {
    console.log(error)
    alert("Não foi possível atualizar")
  }
}
```

**Funcionalidade**:
- Conta o número total de despesas
- Soma todos os valores das despesas
- Formata e exibe o total
- Validação de valores numéricos
- Tratamento de erros

### Remoção de Despesas

```javascript
expenseList.addEventListener("click", function(event) {
  if(event.target.classList.contains("remove-icon")) {
    const item = event.target.closest(".expense")
    item.remove()
    updateTotals()
  }
})
```

**Funcionalidade**:
- Event delegation para capturar cliques
- Identificação do ícone de remoção
- Remoção do elemento pai (.expense)
- Atualização automática dos totais

### Limpeza do Formulário

```javascript
function formClear() {
  expense.value = ""
  category.value = ""
  amount.value = ""
  expense.focus()
}
```

**Funcionalidade**:
- Limpa todos os campos do formulário
- Retorna o foco para o primeiro campo

## 🎯 Conceitos JavaScript Aplicados

### Manipulação do DOM
- `document.querySelector()` e `document.getElementById()`
- `createElement()` e `append()`
- Modificação de classes e atributos
- Event listeners e event delegation

### Tratamento de Eventos
- `onsubmit` para formulários
- `oninput` para inputs em tempo real
- `addEventListener` para eventos de clique

### Manipulação de Strings
- Expressões regulares (`replace(/\D/g, "")`)
- Formatação de moeda (`toLocaleString()`)
- Conversão de tipos (`parseFloat()`, `Number()`)

### Estruturas de Controle
- Loops `for` para iteração
- Condicionais (`if/else`)
- Operador ternário para pluralização

### Tratamento de Erros
- Blocos `try/catch` para captura de erros
- Validação de dados de entrada
- Mensagens de erro para o usuário

### Objetos JavaScript
- Criação de objetos literais
- Propriedades dinâmicas
- Timestamp para IDs únicos

## 🚀 Melhorias Implementadas

- **Formatação em tempo real**: O valor é formatado enquanto o usuário digita
- **Validação robusta**: Verificação de valores numéricos válidos
- **Tratamento de erros**: Try/catch em funções críticas
- **Experiência do usuário**: Foco automático e limpeza de formulário
- **Responsividade**: Interface adaptável a diferentes dispositivos

## 📚 Aprendizados

Este projeto demonstra na prática:
- Manipulação avançada do DOM
- Criação dinâmica de elementos HTML
- Formatação de dados em tempo real
- Tratamento adequado de eventos
- Validação e tratamento de erros
- Organização de código JavaScript
- Boas práticas de desenvolvimento frontend

## 🎨 Interface

A aplicação possui uma interface limpa e intuitiva com:
- Formulário de entrada de dados
- Lista dinâmica de despesas
- Totalizadores automáticos
- Ícones visuais para categorias
- Feedback visual para ações do usuário

Este projeto serve como uma excelente base para compreender os fundamentos do JavaScript moderno aplicado ao desenvolvimento web, demonstrando conceitos essenciais de forma prática e funcional.
