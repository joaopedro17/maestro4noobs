# Organização de Flows

À medida que o projeto cresce, uma boa organização é fundamental para manutenção e escalabilidade.

## Estrutura Recomendada

```
projeto-maestro/
├── flows/
│   ├── autenticacao/
│   │   ├── login-sucesso.yaml
│   │   ├── login-falha.yaml
│   │   └── logout.yaml
│   ├── produtos/
│   │   ├── listar-produtos.yaml
│   │   ├── buscar-produto.yaml
│   │   └── detalhe-produto.yaml
│   ├── carrinho/
│   │   ├── adicionar-item.yaml
│   │   └── remover-item.yaml
│   └── checkout/
│       └── compra-completa.yaml
├── subflows/
│   ├── _login.yaml
│   ├── _logout.yaml
│   ├── _add-to-cart.yaml
│   └── _fechar-popup.yaml
├── scripts/
│   └── setup-dados.js
├── .env.example
├── .env.staging
└── README.md
```

## Convenções de Nomenclatura

| Tipo | Convenção | Exemplo |
|------|-----------|---------|
| Flow principal | `verbo-substantivo.yaml` | `login-sucesso.yaml` |
| Subflow | `_nome.yaml` (com `_`) | `_login.yaml` |
| Script JS | `kebab-case.js` | `criar-usuario.js` |
| Diretório | `kebab-case/` | `autenticacao/` |

## Tags para Categorização

Use tags para categorizar e filtrar flows:

```yaml
# flows/autenticacao/login-sucesso.yaml
appId: com.meuapp.android
name: "Login com credenciais válidas"
tags:
  - smoke
  - autenticacao
  - critico
---
- launchApp
- runFlow: ../../subflows/_login.yaml
- assertVisible: "Dashboard"
```

Executar apenas flows com uma tag específica:

```bash
# Ainda não suportado nativamente — use scripts shell
maestro test flows/autenticacao/
```

## Exemplo: Projeto Completo

```yaml
# flows/checkout/compra-completa.yaml
appId: com.loja.android
name: "Jornada completa de compra"
tags:
  - e2e
  - smoke
  - checkout
---
# Setup
- launchApp:
    clearState: true

# Login
- runFlow:
    file: ../../subflows/_login.yaml
    env:
      EMAIL: ${USUARIO_EMAIL}
      SENHA: ${USUARIO_SENHA}

# Navegar para produtos
- tapOn: "Explorar"

# Adicionar ao carrinho
- runFlow:
    file: ../../subflows/_add-to-cart.yaml
    env:
      PRODUTO: ${PRODUTO_NOME}

# Finalizar compra
- tapOn: "Meu Carrinho"
- tapOn: "Finalizar Compra"
- tapOn: "Confirmar Pedido"

# Verificações
- assertVisible: "Pedido Confirmado!"
- assertVisible: ${PRODUTO_NOME}
```

## Boas Práticas

- Agrupe flows por funcionalidade em subpastas
- Mantenha subflows genéricos e parametrizados
- Use um arquivo `README.md` para documentar o projeto
- Crie um `.env.example` com documentação das variáveis necessárias

---

> Ir para: [Testes de API](../5-Avancado/1-Testes-de-API.md)
