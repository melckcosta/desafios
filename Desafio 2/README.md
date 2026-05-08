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

## Passos adicionais para configurar o projeto Node.js

Além do workflow, é necessário criar os arquivos básicos da aplicação para que o deploy funcione corretamente.

---

### Passo 1 - Criar o arquivo `index.js`

Crie um arquivo chamado `index.js` na raiz do projeto com o seguinte conteúdo:

```javascript
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send(`
    <!DOCTYPE html>
    <html lang="pt-BR">
    <head>
      <meta charset="UTF-8">
      <title>Hello World</title>
      <style>
        body {
          margin: 0;
          font-family: 'Segoe UI', sans-serif;
          background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
          color: #fff;
          display: flex;
          justify-content: center;
          align-items: center;
          height: 100vh;
          text-align: center;
        }
        h1 {
          font-size: 4em;
          margin-bottom: 0.2em;
          text-shadow: 2px 2px 8px rgba(0,0,0,0.3);
        }
        p {
          font-size: 1.5em;
          margin-top: 0;
        }
        .btn {
          display: inline-block;
          margin-top: 20px;
          padding: 12px 24px;
          background: #fff;
          color: #2575fc;
          border-radius: 25px;
          text-decoration: none;
          font-weight: bold;
          transition: 0.3s;
        }
        .btn:hover {
          background: #2575fc;
          color: #fff;
        }
      </style>
    </head>
    <body>
      <div>
        <h1>Hello, World!</h1>
      </div>
    </body>
    </html>
  `);
});

app.listen(port, () => {
  console.log(`Servidor rodando na porta ${port}`);
});
```

### Passo 2 - Criar o arquivo package.json

Crie um arquivo chamado **package.json** na raiz do projeto com o seguinte conteúdo:

```json
{
  "name": "hello-world-node",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Nenhum teste definido\" && exit 0"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

## Resultado esperado

Ao realizar um **push** para a branch `main`, o workflow irá:

1. Fazer o build do projeto Node.js.
2. Criar o pacote da aplicação.
3. Publicar automaticamente no Azure App Services.

Esse desafio demonstra como executar o workflow através de um evento e o fluxo de build/deploy.
Você poderá acessar a aplicação pelo endereço do seu App Service.