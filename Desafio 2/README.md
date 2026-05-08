# Desafio 2 - Build e Deploy de Node.js no Azure App Services

Neste desafio você irá configurar um workflow para **buildar** e **deployar** um projeto Node.js no **Azure App Services**.

---

## Objetivo

- Fazer build de um projeto Node.js.
- Publicar automaticamente no Azure App Services.

---

## Pré-requisitos

- Conta no [Azure](https://portal.azure.com/).
- Um App Service criado para hospedar a aplicação.
- Configurar as **Secrets** no repositório GitHub:
  - `AZURE_WEBAPP_NAME` → Nome do App Service.
  - `AZURE_CREDENTIALS` → Permissão para autenticar na Azure.

---

## Passos

1. Crie o workflow em `.github/workflows/main.yaml`:

```yaml
name: Workflow Node.js App

on:
  push:
    branches: [ "main" ]

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

      - name: Instalar dependências
        run: npm install

      - name: Rodar testes
        run: npm test

      - name: Run build (optional)
        run: npm run build --if-present

      - name: Archive production package
        run: zip -r release.zip .
      
      - name: Upload artifact
        uses: actions/upload-artifact@v7
        with:
          name: node-app
          path: release.zip

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Baixar pacote do GitHub Artifacts
        uses: actions/download-artifact@v8
        with:
          name: node-app

      - name: Extract package
        run: unzip release.zip

      - name: Login na Azure
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - name: Deploy para Azure WebApp
        uses: azure/webapps-deploy@v3
        with:
          app-name: ${{ secrets.AZURE_WEBAPP_NAME }}
          package: .
```

## Resultado esperado

Ao realizar um **push** para a branch `main`, o workflow irá:

1. Fazer o build do projeto Node.js.
2. Criar o pacote da aplicação.
3. Publicar automaticamente no Azure App Services.

Esse desafio demonstra como executar o workflow através de um evento e o fluxo de build/deploy.
Você poderá acessar a aplicação pelo endereço do seu App Service.