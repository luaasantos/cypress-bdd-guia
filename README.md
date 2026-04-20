# 🧪 Integração BDD com Cypress (Gherkin + Cucumber)

![Cypress](https://img.shields.io/badge/Cypress-Testes-green)
![BDD](https://img.shields.io/badge/BDD-Gherkin-blue)
![Cucumber](https://img.shields.io/badge/Cucumber-Integração-brightgreen)
![Status](https://img.shields.io/badge/Status-Em%20evolução-yellow)

---

## 📌 Contexto

Este guia mostra como integrar testes automatizados utilizando **BDD (Behavior Driven Development)** com **Cypress**, utilizando **Gherkin + Cucumber**.

A proposta é tornar os testes:

- Mais legíveis 📖  
- Mais próximos da regra de negócio 🧠  
- Mais reutilizáveis 🔁  

---

## ⚙️ Instalação

Instale as dependências necessárias:

```bash
npm install --save-dev @badeball/cypress-cucumber-preprocessor
npm install --save-dev @bahmutov/cypress-esbuild-preprocessor
```

---

## ⚙️ Configuração
No arquivo cypress.config.js:

```javascript

const { defineConfig } = require("cypress");
const createBundler = require("@bahmutov/cypress-esbuild-preprocessor");
const addCucumberPreprocessorPlugin = require("@badeball/cypress-cucumber-preprocessor").addCucumberPreprocessorPlugin;
const createEsbuildPlugin = require("@badeball/cypress-cucumber-preprocessor/esbuild");

module.exports = defineConfig({
  e2e: {
    specPattern: "**/*.feature",

    async setupNodeEvents(on, config) {
      await addCucumberPreprocessorPlugin(on, config);

      on(
        "file:preprocessor",
        createBundler({
          plugins: [createEsbuildPlugin.default(config)],
        })
      );

      return config;
    },
  },
});
```

## 📁 Estrutura de Pastas

cypress/
 └── e2e/
      └── auth/
           ├── login.feature
           └── login.steps.js

## 🧪 Exemplo de Feature (Gherkin)

```gherkin
Feature: Login

Scenario: Login com sucesso
  Given que o usuário está na página de login do ParaBank
  When informa usuário "valido"
  And informa senha "valida"
  And clica no botão "Log In"
  Then deve acessar a conta
```
## ⚙️ Exemplo de Step Definition

```javascript
import { Given, When, Then } from "@badeball/cypress-cucumber-preprocessor";

Given("que o usuário está na página de login do ParaBank", () => {
  cy.visit("https://parabank.parasoft.com/parabank/index.htm");
});

When("informa usuário {string}", (tipoUsuario) => {
  const usuarios = {
    valido: "john",
    invalido: "fakeUser"
  };

  cy.get('input[name="username"]')
    .clear()
    .type(usuarios[tipoUsuario]);
});

When("informa senha {string}", (tipoSenha) => {
  const senhas = {
    valida: "demo",
    invalida: "123"
  };

  cy.get('input[name="password"]')
    .clear()
    .type(senhas[tipoSenha]);
});

When("clica no botão {string}", (botao) => {
  cy.get(`input[value="${botao}"]`).click();
});

Then("deve acessar a conta", () => {
  cy.contains("Accounts Overview").should("be.visible");
});
```
## ▶️ Como rodar os testes
Abrir interface do Cypress:

```Bash
npx cypress open
```
Rodar em modo headless:
```Bash
npx cypress run
```

## ⚠️ Pontos importantes

- O Gherkin deve ser idêntico aos steps (BDD é literal)
- Evite duplicação → use parametrização
- Mantenha steps reutilizáveis
- Organize bem a estrutura (feature + steps juntos)

## 🚀 Resultado
Com essa abordagem você ganha:

✅ Testes mais legíveis
✅ Melhor organização
✅ Menos código duplicado
✅ Maior alinhamento com o comportamento do usuário

## 💡 Observação
Este guia faz parte da evolução de um projeto real utilizando o ParaBank.
Os commits do repositório mostram na prática a transição de uma automação mais direta para uma abordagem baseada em BDD.

## 🤝 Contribuição
Sinta-se à vontade para explorar, adaptar e evoluir este guia 🚀