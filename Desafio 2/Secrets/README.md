## Configuração do Secret AZURE_CREDENTIALS

Para que o GitHub Actions consiga autenticar e realizar o deploy no Azure App Services, é necessário criar e configurar o secret **AZURE_CREDENTIALS** no repositório.

---

### Passo 1 - Instalar Azure CLI
Baixe e instale o [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=winget) em sua máquina.

---

### Passo 2 - Login na Azure
No terminal, execute:
```bash
az login
```
Autentique-se com sua conta Azure. Esse comando abrirá o navegador para login.

---

### Passo 3 - Criar Service Principal
Crie um Service Principal com permissões de Contributor para sua assinatura:
```bash
az ad sp create-for-rbac --name <nome> --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>
```

---

### Passo 4 - Copiar JSON gerado
O comando acima retorna um JSON semelhante a este:
```json
{
  "appId": "xxxx-xxxx-xxxx",
  "displayName": "nome",
  "password": "xxxx-xxxx-xxxx",
  "tenant": "xxxx-xxxx-xxxx"
}
```

---

### Passo 5 - Montar o AZURE_CREDENTIALS
Monte o JSON no formato esperado pelo GitHub Actions:
```json
{
  "clientId": "APP_ID",
  "clientSecret": "PASSWORD",
  "subscriptionId": "SUBSCRIPTION_ID",
  "tenantId": "TENANT"
}
```

APP_ID → valor de appId retornado.
PASSWORD → valor de password.
SUBSCRIPTION_ID → obtido com az account show.
TENANT → valor de tenant.

---

### Passo 6 - Salvar como Secret no GitHub

1. Vá em **Settings → Secrets and variables → Actions → New repository secret**.  
2. Nomeie o secret como **AZURE_CREDENTIALS**.  
3. Cole o JSON montado.  
4. Salve.

Agora o workflow poderá autenticar na Azure usando o secret **AZURE_CREDENTIALS**.