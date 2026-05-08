# Desafio 1 - Variáveis de Repositório no GitHub Actions

Neste desafio você irá configurar um workflow para **ler variáveis de repositório** e exibir seus valores no log da execução.

---

## Objetivo

- Criar um workflow simples.
- Ler três variáveis configuradas no repositório.
- Exibir os valores no log da execução.

---

## Pré-requisitos

- Repositório no GitHub.
- Configurar as **Repository Variables** em **Settings → Secrets and variables → Actions → Variables**:
  - `VAR1` → Primeira variável.
  - `VAR2` → Segunda variável.
  - `VAR3` → Terceira variável.

---

## Passos

1. Crie o workflow em `.github/workflows/variables.yaml`:

```yaml
name: Workflow Variáveis

on:
  workflow_dispatch:

jobs:
  show-variables:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout código
        uses: actions/checkout@v3

      - name: Exibir variáveis
        run: |
          echo "Valor da VAR1: ${{ vars.VAR1 }}"
          echo "Valor da VAR2: ${{ vars.VAR2 }}"
          echo "Valor da VAR3: ${{ vars.VAR3 }}"

```

## Resultado esperado

Ao executar o **workflow_dispatch** manualmente pelo GitHub, o workflow irá:

1. Ler as variáveis configuradas no repositório.
2. Exibir os valores diretamente no log da execução.

Esse desafio demonstra como utilizar variáveis em workflows do GitHub Actions e como dispará-los manualmente.