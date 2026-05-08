# Desafio 3 - Action Composite para Node.js

Neste desafio você irá criar uma **Action composite** em um repositório separado, que será responsável por instalar dependências, rodar testes e realizar o build de um projeto Node.js utilizando o **npm**.

---

## Objetivo

- Criar uma Action composite reutilizável.
- Automatizar os passos de instalação, teste e build.
- Utilizar essa Action em outros workflows.
- Gerar tags para versionar a Action.

---

## Pré-requisitos

- Novo repositório público no GitHub para hospedar a Action composite.
- Ter concluído o Desafio 2.

---

## Passos

1. No repositório da Action composite, crie o arquivo `action.yml` na raiz:

```yaml
name: "Node.js Composite Action"
description: "Instala dependências, roda testes e builda projeto Node.js"
author: "Seu Nome"

runs:
  using: "composite"
  steps:
    - name: Instalar dependências
      run: npm install
      shell: bash

    - name: Rodar testes
      run: npm test
      shell: bash

    - name: Build do projeto
      run: npm run build --if-present
      shell: bash
```
2. Gere uma tag para versionar a Action:
```bash
git tag v1
git push origin v1
```
3. No repositório do desafio 2, remova as steps que executam o npm e faça o seguinte apontamento:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout código
        uses: actions/checkout@v3

      - name: Configurar Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          
      - name: Usar Composite Action
        uses: nomeDoUsuario/nomeDoRepositorio@v1

      - name: Archive production package
        run: zip -r release.zip .
      
      - name: Upload artifact
        uses: actions/upload-artifact@v7
        with:
          name: node-app
          path: release.zip
```

## Resultado esperado

Ao realizar um **push** para a branch `main`, o workflow irá:

1. Instalar as dependências do projeto Node.js.
2. Rodar os testes configurados.
3. Realizar o build da aplicação (se existir script de build).
4. Utilizar a Action composite versionada por tag.

Esse desafio demonstra como criar, versionar e reutilizar uma **Action composite** para padronizar processos em múltiplos projetos.