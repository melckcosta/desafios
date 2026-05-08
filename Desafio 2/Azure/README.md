## Passo a Passo - Criar um Serviço de Aplicativo no Azure

Este guia mostra como criar um **App Service** no Azure para hospedar sua aplicação Node.js.

---

### Passo 1 - Acessar o Portal Azure
- Entre no [Portal Azure](https://portal.azure.com/).
- Faça login com sua conta ou crie uma.

---

### Passo 2 - Criar um Recurso
- Clique em **Criar um recurso** no menu lateral.
- Na barra de pesquisa, digite **App Service**.
- Selecione **App Service** e clique em **Criar**.

---

### Passo 3 - Configurar Informações Básicas
- **Assinatura**: selecione sua assinatura ativa.
- **Grupo de Recursos**: crie um novo ou escolha um existente.
- **Nome do Aplicativo**: defina um nome único (será usado como URL, ex: `meuapp.azurewebsites.net`).
- **Publicar**: escolha **Código**.
- **Runtime stack**: selecione **Node.js** e a versão desejada.
- **Sistema Operacional**: escolha **Linux**.
- **Região**: selecione a região.

---

### Passo 4 - Configurar Plano de Serviço
- Crie ou selecione um **Plano de Serviço do App**.
- Defina o nível de preço (Free).

---

### Passo 5 - Revisar e Criar
- Clique em **Revisar + Criar**.
- Verifique as configurações.
- Clique em **Criar** para provisionar o App Service.

---

### Passo 6 - Obter Nome do App Service
- Após a criação, vá até o recurso.
- Copie o **Nome do Aplicativo** (ex: `meuapp`).
- Esse valor será usado no secret **AZURE_WEBAPP_NAME** no GitHub.
- Crie a secret no repositório do Github.

---

Agora você tem um **App Service** configurado e pronto para receber deploys via GitHub Actions.
